## HandlerInterceptor, HandlerMethodArgumentResolver

### Interceptor
* what  
    > Interceptor는 특정 핸들러 그룹에 대해 기존 또는 사용자 정의 인터셉터를 등록하여 각 핸들러 구현을 수정할 필요 없이 공통 전처리 동작을 추가할 수 있도록 한다.
  > 이 매커니즘은 예를 들어 권한 확인과 같은 일반적인 핸들러 동작과 같은 전처리 측면의 넓은 분야에 사용될 수 있다.
  > 주 목적은 반복적인 핸들러 코드를 제거하는 것이다.
  
    DispatcherServlet이 Controller를 호출하기 전, 후에 끼어들어 스프링 컨텍스트 내부에서 컨트롤러에 관한 요청과 응답에 관여한다. 
* how  
  * preHandler() - Controller 실행 전  
  * postHandler() - Controller 실행 후 view Rendering 실행 전  
  * afterCompletion() - view Rendering 이후  
    각 메서드의 반환값이 true이면 다음 체인이 실행되지만, false이면 중단하고 남은 인터셉터와 컨트롤러가 실행되지 않는다.
### ArgumentResolver
* what  
    > Strategy interface for resolving method parameters into argument values in the context of a given request.  
  
  컨트롤러에서 파라미터를 바인딩해주는 역할을 수행한다. 예를 들어 특정 클래스나 특정 어노테이션 등의 요청 파라미터를 수정해야한다거나 또는 
클래스의 파라미터를 조작 혹은 공통적으로 써야하는 파라미터들을 바인딩해주는 역할을 한다.
  
* how  
  간단한 로그인 유저라는 객체를 만든 뒤, 컨트롤러에서 들어오는 요청에 대해 name: "조앤", age: 24를 갖는 로그인 유저를 바인딩하도록 하는 ArgumentResolver를 만들어보자.
  
  ```java
    // 1. loginUser 객체 생성
    public class LoginUser {
      private final String username;
      private final int age;
    
    // getter 및 생성자 생략
    }
  
    //2. CustomHandlerArgumentResolver 등록
    // supportsParameter는 바인딩할 클래스를 지정하고,
    // resolveArgument는 바인딩할 객체를 조작하는 로직을 담는다.
    public class CustomHandlerMethodArgumentResolver implements HandlerMethodArgumentResolver {
      @Override
      public boolean supportsParameter(MethodParameter parameter) {
        return LoginUser.class.isAssignableFrom(parameter.getParameterType());
      }
    
      @Override
      public LoginUser resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer, NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception {
        return new LoginUser("조앤", 24);
      }
    }
  
  // 3. Configuration에 우리가 등록한 CustomHandlerMethodArgumentResolver를 등록해준다.
  @Configuration
    public class WebConfig extends WebMvcConfigurerAdapter {
        @Override
        public void addArgumentResolvers(List<HandlerMethodArgumentResolver> argumentResolvers) {
            argumentResolvers.add(new CustomHandlerMethodArgumentResolver());
         }
    }
  ```
  다음과 같이 설정하면, Controller에서 LoginUser를 받을 시 resolver에서 바인딩 해 둔 조앤, 24살의 로그인 유저가 반환된다.

### 참고 자료
* https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/method/support/HandlerMethodArgumentResolver.html
* https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/HandlerInterceptor.html