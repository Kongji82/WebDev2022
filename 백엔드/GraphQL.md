# GraphQL
GraphQL은 페이스북에서 만든 쿼리 언어입니다.

GraphQL은 애플리케이션 프로그래밍 인터페이스(API)를 위한 쿼리 언어이자 서버측 런타임으로 클라이언트에게 요청한 만큼의 데이터를 제공하는데 우선 순위를 뒵니다.

GraphQL은 API를 더욱 빠르고 유연하며 개발자 친화적으로 만들기 위해 설계되었습니다. REST를 대체할 수 있는 GraphQL은 개발자가 단일 API호출로 다양한 데이터 소스에서 데이터를 끌어오는 요청을 구성할 수 있도록 지원합니다.

또한 GraphQL은 API 유지 관리자에게 기존 쿼리에 영향을 미치지 않고 필드를 추가하거나 폐기 할 수 있는 유연성을 부여합니다. 개발자는 자신이 선호하는 방식으로 API를 빌드할 수 있으며, GraphQL 사양은 API가 예측 가능한 방식으로 작동하도록 보장합니다.

## REST API와 비교
REST API는 URL, Method등을 조합하기 때문에 다양한 Endpoint가 존재합니다. 반면 GraphQL에서는 불러오는 데이터의 종류를 쿼리 조합을 통해서 결정합니다.

![GraphQL](https://tech.kakao.com/files/graphql-stack.png)

![GraphQL](https://tech.kakao.com/files/graphql-mobile-api.png)

출처: https://tech.kakao.com/2019/08/01/graphql-basic/

위 그림처럼, GraphQL API를 사용하면 여러번 네트워크 호출을 할 필요 없이, 한번의 네트워크 호출로 처리 할 수 있습니다.

## GraphQL의 구조
쿼리와 뮤테이션 그리고 응답 내용의 구조는 상당히 직관적입니다. 요청하븐 쿼리문의 구조와 응답 내용의 구조는 거의 일치 합니다.

![GraphQL 쿼리문, 응답 데이터](https://tech.kakao.com/files/graphql-example.png)
GraphQL 쿼리문(좌측), 응답 데이터 형식(우측)
 
GraphQL에서는 쿼리와 뮤테이션을 나눕니다. 
쿼리는 데이터를 읽는데 사용하고, 뮤테이션은 데이터를 변조 하는데 사용합니다.



### 쿼리뮤테이션(query/mutation)

### 스키마/타입(schema/type)

### 리졸버(resolver)

### 인트로스펙션(introspection)


## Reference
https://www.redhat.com/ko/topics/api/what-is-graphql
https://www.redhat.com/ko/topics/api/what-is-graphql