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

### Registed claims : 미리 정의된 클레임
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

 ## Spring boot + Jwt
 spring security 추가
 
 Spring Security는 사용자 정보 (ID/PW) 검증 및 유저 정보 관리 등을 쉽게 사용할 수 있도록 제공한다.

* 스프링 시큐리티는 원래 세션 기반 인증을 사용하기 때문에 JWT와 별개로 생각해야 한다.

build.gradle에 Spring Security 추가
``` gradle
implementation 'org.springframework.boot:spring-boot-starter-security'
```
Jwt dependency 추가
``` gradle
implementation group: 'io.jsonwebtoken', name: 'jjwt', version: '0.9.1'
```

### Jwt Token provider
```java
package com.example.jubging.auth;

import com.example.jubging.DTO.TokenDTO;
import com.example.jubging.common.Exception.CAuthenticationEntryPointException;
import com.example.jubging.Model.User;
import com.example.jubging.Repository.UserRepository;
import com.example.jubging.Service.CustomUserDetailService;
import io.jsonwebtoken.*;
import io.jsonwebtoken.impl.Base64UrlCodec;
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.stereotype.Component;
import org.springframework.transaction.annotation.Transactional;

import javax.annotation.PostConstruct;
import javax.servlet.http.HttpServletRequest;
import java.nio.charset.StandardCharsets;
import java.util.Date;
import java.util.List;

// Jwt Token 생성 및 유효성 검증
@Slf4j
@RequiredArgsConstructor
@Component
public class JwtTokenProvider {     // JWT 토큰을 생성 및 검증 모듈

    @Value("spring.jwt.secret")
    private String secretKey;
    private long accessTokenValidMillisecond = 60 * 60 * 1000L * 24 ; // 1일
    private long refreshTokenValidMillisecond = 14 * 24 * 60 * 60 * 1000L;      // 14 days
    private final UserRepository userRepository;

    private final CustomUserDetailService userDetailsService;

    @PostConstruct
    protected void init() {
        // 암호화
        secretKey = Base64UrlCodec.BASE64URL.encode(secretKey.getBytes(StandardCharsets.UTF_8));
    }

    // Jwt 토큰 생성
    public TokenDTO createToken(Long userId, List<String> roles) {
        Claims claims = Jwts.claims().setSubject(String.valueOf(userId));
        claims.put(ROLES, roles);

        Date now = new Date();

        String accessToken = Jwts.builder()
                .setHeaderParam(Header.TYPE, Header.JWT_TYPE)
                .setClaims(claims)  // 데이터
                .setIssuedAt(now)   // 토큰 발행일자
                .setExpiration(new Date(now.getTime() + accessTokenValidMillisecond))     // 토큰 유효시간 설정
                .signWith(SignatureAlgorithm.HS256, secretKey)      // 암호화 알고리즘, 암호키
                .compact();

        String refreshToken = Jwts.builder()
                .setHeaderParam(Header.TYPE, Header.JWT_TYPE)
                .setExpiration(new Date(now.getTime() + refreshTokenValidMillisecond))
                .signWith(SignatureAlgorithm.HS256, secretKey)
                .compact();

        return TokenDTO.builder()
                .grantType("Bearer")
                .accessToken(accessToken)
                .refreshToken(refreshToken)
                .accessTokenExpireDate(accessTokenValidMillisecond)
                .build();
    }

    // Jwt 토큰으로 인증 정보 조회
    @Transactional
    public Authentication getAuthentication(String token){
        // Jwt 에서 claims 추출
        Claims claims = parseClaims(token);

        // 권한 정보가 없음
        if (claims.get(ROLES) == null) {
            throw new CAuthenticationEntryPointException();
        }

        UserDetails userDetails = userDetailsService.loadUserByUsername(claims.getSubject());
        return new UsernamePasswordAuthenticationToken(userDetails, "", userDetails.getAuthorities());
    }

    // Jwt 토큰 복호화해서 가져오기
    private Claims parseClaims(String token) {
        try {
            return Jwts.parser().setSigningKey(secretKey).parseClaimsJws(token).getBody();
        } catch (ExpiredJwtException e) {
            return e.getClaims();
        }
    }

    // Jwt 토큰에서 회원 구별 정보 추출
    private String getUserPk(String token) {
        return Jwts.parser().setSigningKey(secretKey).parseClaimsJws(token).getBody().getSubject();
    }

    // Request의 Header에서 token 파싱 : "X-AUTH-TOKEN: jwt"
    public String resolveToken(HttpServletRequest req) {
        return req.getHeader("X-AUTH-TOKEN");
    }

    public Long getUserId(HttpServletRequest request){
        String token = this.resolveToken(request);
        Long userId = Long.parseLong(this.getUserPk(token));

        return userId;
    }

    public User getUser(HttpServletRequest request){
        String token = this.resolveToken(request);
        User user = (User) this.getAuthentication(token).getPrincipal();

        return user;
    }

    // Jwt 토큰의 유쇼성 + 만료일자 확인
    public boolean validateToken(String token) {
        try {
            Jwts.parser().setSigningKey(secretKey).parseClaimsJws(token);
            return true;
        } catch (SecurityException | MalformedJwtException e) {
            log.error("잘못된 Jwt 서명입니다.");
        } catch (ExpiredJwtException e) {
            log.error("만료된 토큰입니다.");
        } catch (UnsupportedJwtException e) {
            log.error("지원하지 않는 토큰입니다.");
        } catch (IllegalArgumentException e) {
            log.error("잘못된 토큰입니다.");
        }
        return false;
    }
}

```

