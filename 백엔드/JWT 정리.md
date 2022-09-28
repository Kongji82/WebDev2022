# 9월 22일(목)
## JWT(Json Web Token)란?
JWT는 유저를 인증하고 식별하기 위한 토큰(Token)기반 인증이다.

JWT가 가지는 특징으로는 '토큰 자체에 사용자의 권한 정보나 서비스를 사용하기 위한 정보가 포함(self-contained)된다는 것이다.

JWT를 사용하면 다음과 같은 순서로 작동한다.
1. 클라이언트 사용자가 아이디, 패스워드를 통해 웹 서비스 인증
2. 서버에서 서명된(signed) JWT를 생성하여 클라이언트에 응답으로 돌려주기
3. 클라이언트가 서버에 데이터를 추가적으로 요구할 때 JWT를 HTTP Header에 첨부.
4. 서버에서 클라이언트로부터 온 JWT를 검증

## JWT 구조
JWT는 각각의 구성요소가 점(.)으로 구분이 되어있으며 구성요소는 다음과 같다
- Header
- Payload
- Signature

## 1. Header
헤더에는 보통 토큰의 타입이나, 서명 생성에는 어떤 알고리즘이 사용되었는지 저장한다.

## 2. Payload
payload에는 Claim이라는 사용자에 대한, 혹은 토큰에 대한 property를 key-value의 형태로 저장

Claim은 토큰에서 사용할 정보의 조각같은 개념
claim에 어떤 정보를 넣을지는 표준을 참고해서 넣는다.

## Registed claims : 미리 정의된 클레임.
- iss(Issuer; 발행자)
- sub(Subject; 제목)
- aud(Audience; 토큰 대상자)
- nbf(Not Before; 토큰 활성 날짜)
- exp(Expireation time; 만료 시간)
- iat(Issued At; 발행 시간)
- jti(JWI ID; Jwt 토큰 식별자)

## 3. Signature
 Header, Payload 를 Base64 URL-safe Encode 를 한 이후 Header 에 명시된 해시함수를 적용하고, 개인키(Private Key)로 서명한 전자서명이 담겨있다.

전자서명에는 비대칭 암호화 알고리즘을 사용하므로 암호화를 위한 키와 복호화를 위한 키가 다르다. 암호화(전자서명)에는 개인키를, 복호화(검증)에는 공개키를 사용한다.

 