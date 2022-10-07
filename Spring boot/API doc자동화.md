# Api document
API Documents는 API 사용법을 알려주는 문서이다. 쉽게 말하면 매뉴얼이다. 공개된 API는 이런 문서들이 제공이 되어야 사용자들이 쓸 수 있기 때문에 필수로 작성해야 하는 문서이다. 

## 자동화 도구
하지만, API 스펙을 문서로 관리하는것은 상당히 귀찮다. API에 변경이 생겼을 때마다 찾아서 고쳐야 하며, 모두 실제로 한 번씩 확인해보며 작성을 해야 하기 때문이다. 그렇기 때문에 이런 문서화를 자동화할 수 있는 도구들이 생겼다. 그중 주로 사용되는 도구들은 Swagger와 Spring Rest Docs 가 있다.

## Swagger3
실습으로 사용할 라이브러리는 swagger3로 사용한다.

### Gradle 의존성 추가
build.gradle에 포함
``` gradle
implementation group: 'org.springdoc', name: 'springdoc-openapi-ui', version: '1.6.11'
```

### application 파일 설정
``` yaml
springdoc:
  api-docs:
    groups:
      enabled: true
  swagger-ui:
    path: /swagger-ui.html
    displayRequestDuration: true
    groups-order: DESC
```

## OpenAPI bean 등록
Open API bean등록을 위해 Configuration 클래스를 생성한다.
OpenAPI bean에 관한 내용 역시 가장 기본적인 title, version, description에 대한 것만 담았으며, @Value 어노테이션을 통해 .properties에서 등록한 springdoc.version 값을 가지고 오는 것을 확인할 수 있습니다
``` java
@Configuration
public class OpenApiConfig {

  @Bean
  public OpenAPI openAPI(@Value("${springdoc.version}") String springdocVersion) {
    Info info = new Info()
        .title("타이틀 입력")
        .version(springdocVersion)
        .description("API에 대한 설명 부분");

    return new OpenAPI()
        .components(new Components())
        .info(info);
  }
}
```
## Controller 설정
### @Tag

api 그룹 설정을 위한 어노테이션입니다.

name 속성으로 태그의 이름을 설정할 수 있고, description 속성으로 태그에 대한 설명을 추가할 수 있습니다.

@Tag에 설정된 name이 같은 것 끼리 하나의 api 그룹으로 묶게 됩니다.

 

### @Operation

api 상세 정보 설정을 위한 어노테이션입니다.

summary 속성으로 api에 대한 간략한 설명, description 속성으로 api에 대한 상세 설명을 추가할 수 있으며, responses, parameters 속성 등을 추가로 적용할 수 있습니다.

 

### @ApiResponses

바로 아래에서 설명하는 여러 개의 @ApiResponse를 묶기 위한 어노테이션입니다.

 

### @ApiResponse

api의 response 설정을 위한 어노테이션입니다.

responseCode 속성으로 http 상태 코드를 설정할 수 있고, description으로 response에 대한 설명을 추가할 수 있습니다. @ApiResponse 설정을 통해 응답 결과로 나올 수 있는 response 구조를 미리 확인할 수 있게 됩니다. 

api 조회 성공 및 실패 시 발생될 상태 코드 및 Response 구조를 설정하고자 할 때 유용하게 사용될 수 있습니다.

(implementation에는 responseBody로 제공될 클래스 타입만 설정할 수 있습니다.)

 

### @Parameter

예시에는 없지만 해당 어노테이션은 api parameter 설정을 위한 어노테이션입니다.

name으로 파라미터의 이름, description으로 설명, in으로 파라미터의 위치를 설정할 수 있습니다.

 ``` java
@Tag(name = "인증", description = "인증 관련 api 입니다.")
@RestController
@RequestMapping("/api/auth")
public class AuthController {

    @Operation(summary = "로그인 메서드", description = "로그인 메서드입니다.")
    @ApiResponses(value = {
        @ApiResponse(responseCode = "200", description = "successful operation", content = @Content(schema = @Schema(implementation = LoginResponse.class))),
        @ApiResponse(responseCode = "400", description = "bad request operation", content = @Content(schema = @Schema(implementation = LoginResponse.class)))
    })
    @PostMapping(value = "login")
    public ResponseEntity<LoginResponse> login(LoginRequest loginRequest) {
        LoginResponse loginResponse = new LoginResponse("accessTokenValue", "refreshTokenValue");
        return ResponseEntity.ok().body(loginResponse);
    }
}
 ```




## Reference
https://alwaysone.tistory.com/entry/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8%EB%A1%9C-API-%EB%A7%8C%EB%93%A4%EA%B8%B0-Spring-Rest-Docs%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%84%9C-API-Docs-%EC%9E%90%EB%8F%99%EC%9C%BC%EB%A1%9C-%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0