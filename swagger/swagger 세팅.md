## swaager 세팅
```JAVA
package com.umc.thegoods.config;

import io.swagger.v3.oas.models.Components;
import io.swagger.v3.oas.models.OpenAPI;
import io.swagger.v3.oas.models.info.Info;
import io.swagger.v3.oas.models.security.SecurityRequirement;
import io.swagger.v3.oas.models.security.SecurityScheme;
import io.swagger.v3.oas.models.servers.Server;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SwaggerConfig {

    @Bean
    public OpenAPI TheGoodsAPI() {
        Info info = new Info()
                .title("TheGoods API")
                .description("TheGoods API 명세서")
                .version("1.0.0");

        String jwtSchemeName = "JWT TOKEN";
        // API 요청헤더에 인증정보 포함
        SecurityRequirement securityRequirement = new SecurityRequirement().addList(jwtSchemeName);
        // SecuritySchemes 등록
        Components components = new Components()
                .addSecuritySchemes(
                        jwtSchemeName,
                        new SecurityScheme()
                                .name(jwtSchemeName)
                                .type(SecurityScheme.Type.HTTP) // HTTP 방식
                                .scheme("bearer")
                                .bearerFormat("JWT"));

        return new OpenAPI()
                .addServersItem(new Server().url("/"))
                .info(info)
                .addSecurityItem(securityRequirement)
                .components(components);
    }
}

```

- 기본 swagger 코드이다. 기본적으로 spring bean에 등록하여 사용하는 것을 볼 수 있다.
- 추가로 테스트 단계까지 거치면 프로젝트 세팅이 완료된다.

## swagger 테스트

```JAVA

```

## version 추가

```JAVA
@Bean
public GroupedOpenApi publicApi() {
    return GroupedOpenApi.builder()
        .group("v1-definition")
        .packagesToScan("briefing")
        .build();
}
```

- 위와 같이 swaager version 관리도 가능하다.
