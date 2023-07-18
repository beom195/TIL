# JWT

## jwt(Json Web Token)란 무엇인가?

jwt는 사용자를 인증하고 식별하기 위한 토큰 기반 인증이다.  
세션과 달리 서버가 아닌 클라이언트에 저장되어 서버의 부담을 덜어주고  
토큰 자체에 서비스를 사용하기 위한 사용자의 권한과 정보가 포함된다.

> 세션과 쿠키에 대해서는 추후에 알아보도록하자.

## jwt의 구조

jwt는 헤더(header), 페이로드(payload), 서명(signature)  
세 부분으로 나눠지며 `.` 기호로 구분된다.
```
(헤더).(페이로드),(서명)
```
### 헤더(header)
헤더에는 토큰에서 사용될 타입과 해시 암호화 알고리즘으로 구성되어 있다
```
{ 
 "alg": "HS256",
 "typ": "JWT"
}
```
- alg: jwt를 서명하는데 사용한 알고리즘을 지정한다. 보통 HMAC, SHA256 혹은 RSA가 사용된다.
- typ: 토큰의 타입을 지정한다.

### 페이로드(payload)
페이로드에는 claim이라는 사용자 또는 토큰에 대한 정보가 key-value의 형태로 저장되어있다.  
claim은 `Registered Claim`,`Public Claim`,`Private Claim` 세 가지가 있다.

- registered claim: 토큰 정보를 표현하기 위해 미리 정해진 데이터들
  - iss: 토큰 발급자(issuer)
  - sub: 토큰 제목(subject)
  - aud: 토큰 대상자(audience)
  - exp: 토큰 만료 시간(expiration), NumericData 형식
  - nbf: 토큰 활성일(not before), 이 날이 지나기 전의 토큰은 활성화 안됌
  - iat: 토큰 발급 시간(issued at): 토큰 발급 이후의 경과 시간을 알 수 있음
  - jti: JWT 토큰 식별자(JWT ID), 중복 방지를 위해 사용하며 일회용 토큰(Access Token)등에 사용된다.
- public claim: 사용자가 마음대로 정의할 수 있고 충돌을 방지하기 위해서는  IANA JSON에 정의 되어 있거나 네임스페이스를 포함하는 url로 정의해야 한다.

[IANA JSON](https://www.iana.org/assignments/jwt/jwt.xhtml)

```
{
"picture": "this@is.example",
"birthdate": "http://this.is/public_claim/example",
"http://beom195.github.com": true
}
```
위 처럼 등록된 클레임 `picture`, `birthdate` 등을 사용해도 되고  
url 형태로 사용할 수 있다.

- private claim: 클라이언트와 서버간에 협의된 클레임 이름을 사용하고 registerd claim과 충돌이 일어나지 않게 주의해야한다. 
```
{
"user_id": "11223355"
"user_password": "q1w2e3r4@"
}
```

### 서명(signature)
서명은 토큰을 인코딩, 유효성 검증을 할 때 사용되는 암호화 코드이다.  
헤더와 페이로드의 값을 BASE64로 인코딩 한 후 그 값을 비밀 키를 이용해  
특정 해싱 알고리즘을 적용해 서명이 생성된다.


> jwt 토큰은 네트워크 전송을 하기 때문에 공간을 적게 차지하기 위해  
> 데이터를 저장할 때 키(key)를 3글자로 줄이는 관행이 있다.

## Java 활용 Jwt 토큰 생성 예시

> jjwt 오픈소스 라이브러리를 사용하여 jwt를 생성한 코드들이다.  
> 테스트 코드는 maven을 기준으로 테스트하였다.

먼저 pom.xml 파일에 jjwt 의존성을 추가 해준다.
```xml
<dependencies>
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt-api</artifactId>
        <version>0.11.2</version>
    </dependency>
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt-impl</artifactId>
        <version>0.11.2</version>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt-jackson</artifactId>
        <version>0.11.2</version>
        <scope>runtime</scope>
    </dependency>
</dependencies>

```

jjwt 라이브러리는 보안 상의 이유로 256비트 이상의 안전한 비밀키를 권장한다.  
비밀키는 충분히 길고 난수로 구성되어야하기 때문에  
'SecureRandom' 클래스를 사용하여 안전한 난수를 생성한다. 


```java
public class JwtTokenGenerator {

    public static void main(String[] args) {
        // 비밀키 생성
        byte[] mySecretKey = new byte[32]; // 256비트 (32바이트)
        SecureRandom secureRandom = new SecureRandom();
        secureRandom.nextBytes(mySecretKey);
        String secretKey = Base64.getEncoder().encodeToString(mySecretKey);
      
        // 생성된 비밀키 출력
        System.out.println("Generated Secret Key: " + secretKey);
    }
```
토큰의 만료 시간을 설정해준다.
```java
        // 토큰의 만료 시간 설정
        long expirationTimeMillis = 3600000; // 1시간

        // 현재 시간과 만료 시간으로부터 만료 일자 계산
        Date now = new Date();
        Date expirationDate = new Date(now.getTime() + expirationTimeMillis);
```
헤더와 페이로드는 json 형식으로 구성되어야한다.  
jjwt 라이브러리의 Jwts.builder()를 사용해 헤더와 페이로드를 설정 할 수 있다.  
.signWith() 메서드를 사용하여 위에서 생성했던 secretKey와 연결하여  
jwt 토큰을 생성한다.
```java
// JWT 토큰 생성
        String token = Jwts.builder()
                .setHeaderParam("alg", "HS256") // 헤더 설정
                .setHeaderParam("typ","JWT")
                .setSubject("JWT_TEST") // 페이로드 설정
                .claim("user_name","jinbeom")
                .claim("user_age", "25")
                .claim("user_email","qwer@abc.com")
                .claim("http://github.com","true")
                .setIssuedAt(now) // 토큰 발급 시간 설정
                .setExpiration(expirationDate) // 토큰 만료 시간 설정
                .signWith(SignatureAlgorithm.HS256, secretKey) // 서명 알고리즘과 비밀키 설정
                .compact();

        // 생성된 토큰 출력
        System.out.println("Generated Token: " + token);
    }
}
```
코드를 실행하면 토큰이 생성 되는걸 볼 수 있다.
``` 
Generated Token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJKV1RfVEVTVCIsInVzZXJfbmFtZSI6ImppbmJlb20iLCJ1c2VyX2FnZSI6IjI1IiwidXNlcl9lbWFpbCI6InF3ZXJAYWJjLmNvbSIsImh0dHA6Ly9naXRodWIuY29tIjoidHJ1ZSIsImlhdCI6MTY4OTY4MTA5OSwiZXhwIjoxNjg5Njg0Njk5fQ.aOhUsEmn15XBUxM40opeM4GVtEI2bgXo4cGIld_8hsQ
```
생성된 토큰을 [jwt.io](https://jwt.io/)에 들어가서 확인해보면  
<img src="/ETC/img/jwt_test.jpg">

페이로드 값에 위에서 설정한 값들이 나오는걸 확인 할 수 있다.

## 마무리
jwt의 구조인 헤더, 페이로드, 서명의 기본적인 개념을 알게 되었고  
테스트 코드를 활용해 직접 토큰까지 생성하는 좋은 경험이 되었다.  
추후에는 jwt와 비교되는 쿠키와 세션에 대해서 알아보는 시간을 가져보도록하겠다.

## 참고
- https://www.daleseo.com/jwt/  
- https://velog.io/@dnjscksdn98/JWT-JSON-Web-Token-%EC%86%8C%EA%B0%9C-%EB%B0%8F-%EA%B5%AC%EC%A1%B0
- https://erjuer.tistory.com/87