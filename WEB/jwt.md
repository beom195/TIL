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
헤더에는 토큰에서 사용될 타입과 해시 암호화 알고리즘으로 구성되어 있다
```
{ 
 "alg": "HS256",
 "typ": JWT
}
```
- alg: jwt를 서명하는데 사용한 알고리즘을 지정한다. 보통 HMAC, SHA256 혹은 RSA가 사용된다.
- typ: 토큰의 타입을 지정한다.

> 이후 내용 업데이트 예정



