# API 응답 통일

💁🏻 Why?

보통 API 자체도 함수처럼 호출해서 하나의 변수에 그 응답을 받는데 그 응답의 형태가 제각각이면 혼란스럽다.

▶️ 프로젝트 진행에 있어서 API 응답 통일은 필수!

- 보통 API 응답 형태는 아래의 형태와 같다.

```json
{
	isSuccess : Boolean
	code : String
	message : String
	result : {응답으로 필요한 또 다른 json}
}
```
- **code**: HTTP 상태 코드 외에 더 세부적인 결과를 알려주기 위해 사용
- **message**: code에 추가적으로 어떤 결과인지 알려주기 위해 사용
- 실패한 경우는 result가 딱히 필요 없어서 null을 준다.

---

## 🔷 1.  API 응답을 나타내는 클래스 정의

- 클래스 명은 ApiResponse, 코드는 아래와 같다.

```JAVA
@Getter
@AllArgsConstructor
@JsonPropertyOrder({"isSuccess", "code", "message", "result"})
public class ApiResponse<T> { // 어떤 타입을 다룰지 모르기 때문에, Generic으로 작성

		@JsonProperty("isSuccess")
    private final Boolean isSuccess;
    private final String code;
    private final String message;
    @JsonInclude(JsonInclude.Include.NON_NULL)
    private T result;


    // 성공한 경우 응답 생성

//    public static <T> ApiResponse<T> onSuccess(T result){
//        return new ApiResponse<>(true, SuccessStatus._OK.getCode() , SuccessStatus._OK.getMessage(), result);
//    }
//
//    public static <T> ApiResponse<T> of(BaseCode code, T result){
	//        return new ApiResponse<>(true, code.getReasonHttpStatus().getCode() , code.getReasonHttpStatus().getMessage(), result);
//    }


    // 실패한 경우 응답 생성
    public static <T> ApiResponse<T> onFailure(String code, String message, T data){
        return new ApiResponse<>(true, code, message, data);
    }
}
```

- ALWAYS : 기본값, 항상 포함
- NON_NULL : 속성값이 null이 아닌 경우에만 포함
- ABSENT : Optional과 함께 사용될 때, 값이 존재하면 포함
- NON_EMPTY : 문자열 or 컬렉션과 같은 경우, 값이 비어있지 않으면 포함

## 🔷 2. 응답에 대한 상세 설명을 위해 Enum 상수 정의

```JAVA
@@Getter
@AllArgsConstructor
public enum CommonErrorStatus implements BaseErrorCode {

    // 가장 일반적인 응답
    _INTERNAL_SERVER_ERROR(HttpStatus.INTERNAL_SERVER_ERROR, "COMMON500", "서버 에러, 관리자에게 문의 바랍니다."),
    _BAD_REQUEST(HttpStatus.BAD_REQUEST,"COMMON400","잘못된 요청입니다."),
    _UNAUTHORIZED(HttpStatus.UNAUTHORIZED,"COMMON401","인증이 필요합니다."),
    _FORBIDDEN(HttpStatus.FORBIDDEN, "COMMON403", "금지된 요청입니다."),
    ;

    // 멤버 관련 응답

    // < > 관련 응답 ...


    private final HttpStatus httpStatus;
    private final String code;
    private final String message;

    @Override
    public ErrorReasonDTO getReason() {
        return ErrorReasonDTO.builder()
                .message(message)
                .code(code)
                .isSuccess(false)
                .build();
    }

    @Override
    public ErrorReasonDTO getReasonHttpStatus() {
        return ErrorReasonDTO.builder()
                .message(message)
                .code(code)
                .isSuccess(false)
                .httpStatus(httpStatus)
                .build()
                ;
    }
}
```

- enum 자신의 값으로 가지고 있던 message, code, httpStatus값을 interface의 메소드 오버라이딩을 통하여 DTO를 만드는 것을 확인할 수 있다.

### 참고(인터페이스)

```java
public interface BaseCode {

    public Reason getReason();

    public Reason getReasonHttpStatus();
}
```

```java
public interface BaseErrorCode {

    public ErrorReason getReason();

    public ErrorReason getReasonHttpStatus();
}
```

- 먼저 두 개의 인터페이스인 BaseCode와 BaseErrorCode의 역할은 **이를 구체화 하는 Status에서 두 개의 메소드를 반드시 Override할 것을 강제**합니다.
