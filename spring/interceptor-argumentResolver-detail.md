## HandlerMethodArgumentResolver

* 주어진 요청의 컨텍스트에서 메서드 매개 변수를 인수값으로 분석하기 위한 전략 인터페이스

그럼, 컨텍스트란 무엇인가?
컨텍스트란 빈(스프링에서 등록하고 관리하는 객체)의 확장 버전으로, Spring이 빈을 좀 더 다루기 쉽도록 기능들이 추가된 공간.
Bean들을 다루기 위한 컨테이너라는 공간이 있는데, 이 공간 내부에 컨텍스트라는 공간이 또 있다.  
즉, 컨텍스트란 빈들을 다루기 위해 우리가 설정할 수 있는 공간