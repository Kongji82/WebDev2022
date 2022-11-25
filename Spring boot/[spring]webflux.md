# Webflux란
Spring Webflux란 spring5에서 새롭게 추가된 모듈이다.
Webflux는 클라이언트, 서버에서 reactive 스타일의 어플리케이션 개발을 도와주는 모듈이며, reactive-stack web framwork이며 non-blocking에 reactive stream을 지원합니다.

장점: 고성능, spring과 완벽한 통합, netty 지원, 비동기 non-blocking 메세지 처리
단점: 오류처리가 다소 복잡하다

## Spring MVC VS Spring Webflux
Spring MVC와 WebFlux의 공통점은 @Controller, Reactive 클라이언트입니다. 둘 다 Tomcat, Jetty, Undertow와 같은 서버에서 실행할 수 있습니다. Spring MVC에서는 명령형 논리, JDBC, JPA를 가질 수 있습니다. Spring WebFlux에서는 기능적 엔드 포인트, 이벤트 루프, 동시성 모델을 가질 수 있습니다. Spring WebFlux는 Netty 서버에서 실행할 수 있다는 장점이 있습니다.

![spring mvc vs spring webflux](https://docs.spring.io/spring-framework/docs/current/reference/html/images/spring-mvc-and-webflux-venn.png)



## Reference
https://devuna.tistory.com/108
https://docs.spring.io/spring-framework/docs/current/reference/html/web-reactive.html#webflux