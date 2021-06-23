## Nginx, Apache
Nginx와 아파치는 모두 웹 서버로서 사용된다.
### Apache
* what
    아파치는 Apache 2.0 오픈 소스 하에서 배포된 웹 서버이다. 여타 다른 웹 서버들처럼 HTML,
  PHP, 미디어 파일 등의 웹 컨텐트를 호스팅하고, 방문한 사용자들에게 해당 컨텐츠를 제공한다. 

* how  
  요청 하나 당 프로세스가 처리하는 구조이다. 자세히 말하자면 클라이언트에서 요청을 받으면 MPM(Multi Processing Module)이라는 방식으로 처리를 하는데, 대표적으로는 Prefork와 Worker 방식이 있다.
  Apache에 WAS로 톰캣을 연동하는 경우라면 Apache 여러 방식을 아파치 자체에서 제공해주기 때문에 다양하고 효율적으로 연동할 수 있다.
  
### Nginx
* what  
    보안과 속도를 최적화시키려는 노력에서 탄생한 웹 서버로서 사용이 매우 간단하고 규모가 작은 서비스이면서 정적 데이터 처리가 많은 서비스에 적합하다.  
  프로그램의 흐름이 이벤트에 의해 결정되는 Event-Driven 방식의 웹 서버이다.
  
* how
    비동기 Event-Driven 기반 구조이므로 CPU와 관계 없이 모든 IO들을 이벤트 리스너로 미루기 때문에 흐름이 끊기지 않고 응답이 빠르게 진행되어 1개의 프로세스로 더 많은 작업을 할 수 있다.

### 참고 자료
* https://taetaetae.github.io/2018/06/27/apache-vs-nginx/  
* https://victorydntmd.tistory.com/231