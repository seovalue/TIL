## REST API 제대로 알기
* what
  HTTP라는 기존 웹 표준을 그대로 사용하는 자원(Resource)과 행위, 표현으로 구성된 API이다. 자원은 URI로 표현되며 행위는 HTTP Method로서 표현할 수 있다. 이를 통해 REST API 메시지만 보고도 원하는 요청을 쉽게 이해할 수 있다.
  추가적으로 URI로 지정한 리소스에 대한 동작을 통일되고 한정적인 인터페이스로 수행하며, 작업을 위한 상태 정보를 저장하거나 관리하지 않는다. 따라서 들어오는 요청만을 단순히 처리한다.

+URI를 이용해 명시된 자원에 접근하고 자원에 어떠한 조작을 할 지 HTTP method를 통해 나타내는 방법이다.

* when
  URI를 통해 명시된 자원에 접근하고 조작에 대한 요청을 보내고 싶을 때 REST를 사용할 수 있다.

* how
1. 정보의 자원을 표현하는 URI를 이용한다.  특히 리소스명은 동사보다는 소문자이며 복수형 명사를 사용한다. `_`는 사용하지 않고 가독성을 위해서는 `-`를 사용한다. 또한 파일 확장자는 URI에 포함시키지 않는다.
   `GET /members/1`
2. 자원에 대한 행위를  적절한 HTTP Method를 통해 표현한다.
   `DELETE /members/1`
   적절한 HTTP Method는 다음과 같은 기준으로 선정할 수 있다.
```
POST - 리소스 생성
GET - 리소스 조회 및 자세한 정보 가져오기
PUT - 리소스 수정
DELETE - 리소스 삭제
```
3. 적절한 HTTP 응답 상태 코드를 통해 응답한다.
   상태 코드는 [HTTP 상태 코드 - HTTP | MDN](https://developer.mozilla.org/ko/docs/Web/HTTP/Status) 에서 확인할 수 있다.

* why
  웹의 장점을 최대한 활용할 수 있는 아키텍처로서 고안되었다. 따라서 HTTP의 장점을 최대한 활용하면서 URI만으로 원하는 요청에 대한 표현을 할 수 있도록 하기 위해 사용한다.

* 참고 - [REST API 제대로 알고 사용하기 : NHN Cloud Meetup](https://meetup.toast.com/posts/92)
