## Cookie와 Session
### Cookie
* what

쿠키는 사이트를 방문하고 이용할 때 브라우저에 저장되는 데이터인데, 웹 서버에서 생성되며, 사용자를 식별할 수 있는 key-value 쌍으로 이루어진 String 형태를 갖는다.

무상태 프로토콜인 HTTP 환경에서 상태를 유지하기 위한 기술이다. 데이터를 브라우저 측에 저장해두고 매 요청시 마다 서버로 쿠키를 전송한다. 따라서 페이지가 새로고침되거나 새로운 요청이 발생된다고 해도 클라이언트측의 브라우저에 데이터가 저장된 상태로 이동된다.

* when - why
1. 최초 페이지에서 로그인하고 여러 페이지에 걸쳐 로그인 인증정보가 유지되어야하는 경우
2. 쇼핑몰과 같은 사이트에서 장바구니에 담긴 아이템 정보들이 다른 페이지로 이동 시에도 유지되어야하는 경우
3. 새로고침과 같은 경우에도 사용자의 상태가 변경되지 않아야하는 경우
4. 이전 단계의 페이지의 정보들이 유지되어야하는 경우
5. 하루 안보기
   -> 지워지거나, 조작되거나, 가로채이더라도 큰 일은 없을 그런 수준의 정보를 브라우저에 정보를 저장할 때 사용.

* how
1. 쿠키 생성하기
```
@RequestMapping(value="/", method=RequestMethod.GET)
public String cookie(HttpServletResponse response){
  Cookie setCookie = new Cookie("cookie", value);
  setCookie.setMaxAge(60*60*24); // 기간을 하루로 지정(60초 * 60분 * 24시간)
  response.addCookie(setCookie); // response에 Cookie 추가
}
```

2. 쿠키 가져오기
```
@RequestMapping(value="/", method=RequestMethod.GET)
public String cookie(HttpServletRequest request){
      Cookie[] getCookie = request.getCookies();
      for(int i=0; i<getCookie.length; i++){
        Cookie c = getCookie[i]; // 객체 생성
        String name = c.getName(); // 쿠키 이름 가져오기
        String value = c.getValue(); // 쿠키 값 가져오기
      }
	 }
}
```

3. 쿠키 제거
```
@RequestMapping(value="/", method=RequestMethod.GET)
public String testCookie(HttpServletRequest request, HttpServletResponse response){
    Cookie[] cookies = request.getCookies(); 
    for(int i=0; i< cookies.length; i++){ 
      cookies[i].setMaxAge(0); // 유효시간을 0으로 설정
      response.addCookie(cookies[i]); // 응답 헤더에 추가
    }
  }
}
```

### 참고 자료
* https://en.wikipedia.org/wiki/HTTP_cookie
* [네트워크 HTTP 쿠키와 세션이란 ? :: 인생의 로그캣](https://noahlogs.tistory.com/38)
* [컨트롤러에서 쿠키 생성, 제거](https://kingle1024.tistory.com/94)

## Session
* what
  세션이란 서버가 해당 서버로 접근한 클라이언트를 식별하는 방법으로 서버는 접근한 클라이언트에게 response-header field인 set-cookie 값으로 클라이언트 식별자인 session-id를 응답한다.

일정 시간 동안 같은 사용자(브라우저)로부터 들어오는 일련의 요구를 하나의 상태로 보고 그 상태를 일정하게 유지시키는 기술이다. 여기서 일정 시간이란 방문자가 웹 서버에 접속해있는 상태를 하나의 단위로 보고 세션이라고 칭한다.

세션은 서버에 그 정보를 저장한다. (쿠키는 클라이언트에 저장시켜 내보냄)

* when
  서비스 관리자가 직접 관리해야할 중요한 정보들은 세션으로 서버 안에서 직접 다루어진다.

* how
1.  클라이언트가 서버로 접속을 요청한다.
2. 서버는 접근한 클라이언트의 request-header field인 cookie를 확인해 클라이언트가 해당 session-id를 보내왔는지 확인한다.
3. 만약 클라이언트로부터 발송된 session-id가 없다면 서버는 session-id를 생성해 클라이언트에게 response-header field인 set-cookie 값으로 session-id를 응답한다.
4. 서버로부터 응답된 세션 아이디는 해당 서버와 클라이언트 메모리에 저장된다.
5. 클라이언트 접속 종료 시 서버에 저장된 세션 아이디는 소멸된다.

### 참고 자료
* [쿠키와 세션이란](https://juyoung-1008.tistory.com/2)
* [무하프로젝트 :: HTTP Session 이란?](https://mohwaproject.tistory.com/entry/HTTP-Session-%EC%9D%B4%EB%9E%80)
