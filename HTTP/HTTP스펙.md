# HTTP 스펙이란?

> 백엔드 개발자라면, 기본적인 HTTP 스펙은 알아야 함 </br>
> - 다음 2가지 정도는 예상 되어야 함
>   - 내가 이 request에서 꺼낼 때, 어떤 http 스펙에서 꺼내는 거구나!
>   - 내가 이 response로 내보낼 때, http 스펙으로 어떻게 나가겠구나!



<aside>
💡 HTTP 스펙은 HTTP(HyperText Transfer Protocol)의 규칙과 구조를 정의한 문서
</aside>

- HTTP는 웹에서 클라이언트(ex. 웹 브라우저)와 서버가 서로 통신할 때 사용하는 프로토콜
- HTTP 스펙은 이러한 통신 과정에서 사용하는 메시지 형식, 메서드, 상태 코드 등을 명확히 정의하여 모든 클라이언트와 서버가 일관성 있게 상호작용할 수 있도록 규격화 한 것을 뜻함

## HTTP 스펙의 주요 요소

---

### 1. 메서드(Method)

- HTTP 메서드는 클라이언트가 서버에 어떤 작업을 요청하는지를 나타냄
- 주요 메서드
    - GET, POST, PUT, DELETE, PATCH
- 예시

  `GET /index.html HTTP/1.1`

  → HTTP/1.1 프로토콜 기반으로 index.html을 조회, 가져와라!


### 2. 요청(Request) + 응답(Response)

**client → server**

```makefile
POST /save HTTP/1.1
Host: localhost:8080
Content-Type: application/x-www-form-urlencoded

username=kim&age=20
```

- Content-Type: application/x-www-form-urlencoded

  → HTTP 통신에서 헤더 중 하나로, 데이터를 URL 인코딩 방식으로 전송한다는 뜻

  → 이 형식은 주로 HTML 폼(form) 데이터를 서버로 전송할 때 사용됨

    - 데이터는 키 - 값 쌍으로 구성되고, 각 쌍은 앰퍼샌드(&)로 구분됨

        ```makefile
        username=kim&age=20
        ```


---

**server → client**

```makefile
HTTP/1.1 200 OK
Content-Type: application/json

{"message": "User saved successfully"}
```

### 3. Headers

- 요청이나 응답의 메타데이터를 포함
- `Content-Type` 헤더는 본문의 데이터 형식을 나타냄
    - `Content-Type: application/json`

### 4. Body(본문)

- 요청이나 응답의 실제 데이터를 포함합니다. GET 요청은 일반적으로 본문을 가지지 않음
- POST나 PUT 요청은 본문에 데이터를 포함해서 요청함
- 예시

  `{"username": "kim", "age": 20}`


### 5. 상태 코드

- 1xx: 정보 응답
- 2xx: 성공
- 3xx: 리다이렉션
- 4xx: 클라이언트 오류
- 5xx: 서버 오류

ex) `200 OK`, `404 Not Found`
