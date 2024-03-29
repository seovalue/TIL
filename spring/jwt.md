## JSON Web Token

* what  
    Json Web Token이라 불리는 JWT는 모바일이나 웹의 사용자 인증을 위해 사용하는 암호화된 토큰을 의미한다.  
  JWT 토큰은 총 세 부분으로 나뉘는 형태를 갖는다. 첫번째 **.**까지는 Header를 의미한다. 헤더는 토큰을 어떻게 해석해야하는 지를 명시한다. 
  두번째 **.**까지는 payload로서, 실제 토큰의 바디이며 토큰에 포함할 내용을 담는다. 마지막은 Signature로서 header-payload 위변조 검증을 위한 부분이다.
  ![jwt-token](https://blog.outsider.ne.kr/attach/1/x4303362580.gif.pagespeed.ic.S7w941_sSQ.webp)
  
* use
    JWT가 다른 토큰과 가장 다른 부분은 토큰 자체가 데이터를 갖고 있다는 점.  
  토큰을 받아서 서명으로 유효한 토큰인지 검증한 뒤, 유효하다고 판단하면 payload를 디코딩해서 토큰에 담긴 데이터를 열어본다. payload에는 만료 시간등이 담겨있으므로 토큰이 사용 가능한지 검사를 하고 이상이 없을 시 바로 사용한다.
  토큰의 사용자 아이디 등이 페이로드에 담겨있다면 db나 캐시를 조회할 필요 없이 애플리케이션 자체에서 사용자를 확인/조회할 수 있다. 
  하지만 다른 토큰보다 길이가 길다.

* 주의사항
    1. payload는 암호화되지 않으므로 최소한의 데이터만을 담아야한다.
    2. jwt token을 강제로 만료시킬 방법이 없다. 토큰 발급 시 해당 토큰의 유효성이 결정되므로, 사용자는 사용을 만료했으나 만료가 안된 상황에서 탈취를 당한다면 만료 시점까지 그 토큰은 유효하다.

### 참고 자료
- https://blog.outsider.ne.kr/1160?commentId=570298#comment570298