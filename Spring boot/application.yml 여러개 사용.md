# application 파일
spring boot에서 application파일은 웹 애플리케이션의 실행 환경에 대한 설정 파일이다.  
서버 포트, db연결 url 등 다양한 설정을 바꿀 수 있도록 제공한다.
그러다보니 로컬 환경과 배포환경에서의 환경설정은 다르기 때문에 매번 바꿔줘야 한다.  
이러한 점을 구분하기 위해 배포버전 application파일과 개발버전 application파일로 나누어 쓴다.

## application.yml
``` yml
server:
  port: 8080

spring:
  profiles:
    active: dev or prod # application 뒤에 붙은 이름
```


## application-dev.yml
``` yml
spring:
  datasource:
    url: [로컬 데이터베이스 주소]
    username: [로컬 데이터베이스 계정]
    password: [로컬 데이터베이스 비밀번호]
  jpa:
    show-sql: true
    hibernate:
      ddl-auto: create

```

## aplication-prod.yml
``` yml
spring:
  datasource:
    url: [배포 데이터베이스 주소]
    username: [배포 데이터베이스 계정]
    password: [배포 데이터베이스 비밀번호]
  jpa:
    show-sql: true
    hibernate:
      ddl-auto: create

```

이와같이 여러개의 설정파일을 만들고 필요에 따라서 기본 application.yml파일에 설정해주면 된다.