## Http와 Https의 차이

### Http
* what  
    서로 다른 시스템들 사이에서 데이터를 주고 받기 위한 가장 기초적인 프로토콜    
    80번 포트를 사용하고 있다.  
    상태를 가지고 있지 않은 Stateless 프로토콜이며, Method, Path, Version, Headers, Body로 구성된다.  
    암호화되지 않은 평문 데이터를 전송하는 프로토콜이다.

### Https
* what  
    HTTP + Secure의 약자이다. 일반 HTTP 프로토콜의 문제점은 서버에서 브라우저로 전송되는 정보가 암호화되지 않는다는 것이었다.
    HTTPS 프로토콜은 SSL을 사용함으로써 이 문제를 해결했다.  
    443번 포트를 사용한다.  
    네트워크 상에서 중간에 제 3자가 정보를 볼 수 없도록 공개키 암호화를 지원하고 있다.
  
개인정보와 같은 민감한 데이터를 주고 받을 때엔 HTTPS, 단순 정보 조회등을 처리 시에는 HTTP.

![http vs https](http://blog.wishket.com/wp-content/uploads/2020/02/03-3.png)

### 참고 자료
- http://blog.wishket.com/http-vs-https-%EC%B0%A8%EC%9D%B4-%EC%95%8C%EB%A9%B4-%EC%82%AC%EC%9D%B4%ED%8A%B8%EC%9D%98-%EB%A0%88%EB%B2%A8%EC%9D%B4-%EB%B3%B4%EC%9D%B8%EB%8B%A4/