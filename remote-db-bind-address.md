## 원격 DB 연결 시 bind-address란?

> Summary
> 
> 통신 요청을 받는 Network Interface Card가 다르며, 소켓 생성 시 각 IP마다 서로 다른 소켓 파일이 생성된다.
> 따라서 mysql.cnf에 작성한 bind-address에 알맞은 ip를 갖는 소켓 파일에서 오는 요청만 허용한다.

애플리케이션 서버와 디비 인스턴스를 분리하면서, 원격으로 DB에 커넥션 연결을 요청해야했다.

그렇게 하기 위해서는 DB 인스턴스에 mysql을 설치하고, mysql.cnf 파일에서 보안 그룹에 알맞은 port와 bind-address를 설정해주어야한다. 그와 관련된 글은 여기 에서 자세히 확인할 수 있다.





우선 DB 인스턴스를 분리하면서 가장 반감이 들었던 부분은 bind-address를 0.0.0.0으로 설정해줌으로서 네트워크 전체 대역에서 오는 요청을 허용해주어야만 커넥션이 가능하다는 점이었다. 처음 bind-address를 이해하기로는 요청이 들어오는 웹 서버의 ip를 적어주어야한다고 생각했다. 그래서 0.0.0.0 대신 시도해본 것은 서버의 private/public ip 모두 bind-address에 할당해보았지만 애초에 mysql.cnf에서 서버의 private/public ip로 bind-address를 작성하는 것이 허용조차 되지 않았다.



하지만 어제, 우테코 크루인 에어가 db 인스턴스의 private ip를 bind-address에 작성해주어도 커넥션이 이루어진다고 알려주었다. 도대체 bind-address는 무엇이길래 db 인스턴스의 private ip를 작성해주면 연결이 되는 것일까?



Bind-address란
bind-address란 데이터베이스 서버가 어떤 주소로의 요청을 허용할 것인지 설정하는 것이다. 그래서 0.0.0.0/0을 설정하게 되면 네트워크 전체 대역, 즉 모든 트래픽에 대해서 요청을 허용한다는 뜻이기에 문제없이 원격 커넥션이 이루어진다.



이 내용을 적용해서, db 인스턴스의 private ip가 1.2.3.4 라고 가정한 뒤 bind-address에 1.2.3.4를 할당한 경우에 대해 생각해보자.

bind-address에 1.2.3.4를 할당했다는 의미는 데이터베이스 서버에 1.2.3.4로 오는 요청을 허용한다는 의미가 된다. 따라서 우리는 서버에서 jdbc:mysql://1.2.3.4:3306/{schema} 로 커넥션을 요청하면 통과되는 것이다.



그런데, bind-address에 127.0.0.1을 할당하나, 인스턴스의 Private ip를 할당하나  jdbc:mysql://1.2.3.4:3306/{schema}로 오는 요청은 둘 모두 성공해야하는 것 아닌가? 라는 의문이 들 수 있다.



127.0.0.1을 바인딩하는 것과 인스턴스의 Private ip를 바인딩하는 것의 차이
요약: 실제 통신 요청을 받는 NIC가 다르고 소켓 생성 시 필요한 정보들이 달라지기 때문에 차이가 발생한다.


우리 서버에는 Network Interface Card라는 것이 있다. NIC에는 하나의 Mac 주소가 있고 이 맥주소에는 하나의 아이피가 할당된다. (아이피는 보통 공유기 혹은 라우터 네트워크 장비로부터 동적 할당된다.) 위 3분 요약에서 소켓 생성 시 필요한 정보들이 달라지기에 차이가 발생한다고 했는데, 소켓 파일은 파일 디스크립터, local ip, local port, remote ip, remote port등 총 5가지의 정보로 생성되는 하나의 파일이다. 따라서 소켓 생성 시 localhost로 요청할 땐 127.0.0.1의 정보를 활용해서 소켓 파일을 생성하고, db 인스턴스의 private ip로 요청할 경우 private ip의 정보를 활용해서 소켓 파일을 생성한다.



즉, bind-address에 127.0.0.1을 할당하면 127.0.0.1 정보를 통해 생성된 소켓 파일을 통해서 온 요청만 mysql 서버에서 허용하겠다는 의미이기에, private-ip로 요청을 보내도 커넥션이 되지 않는 것이다.





참고 자료

-https://dev.mysql.com/doc/refman/5.7/en/ipv6-server-config.html

-http://jkkang.net/unix/netprg/chap2/net2_1.html
