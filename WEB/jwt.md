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





## 참고
- https://www.daleseo.com/jwt/  
- https://velog.io/@dnjscksdn98/JWT-JSON-Web-Token-%EC%86%8C%EA%B0%9C-%EB%B0%8F-%EA%B5%AC%EC%A1%B0