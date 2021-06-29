### ControllerAdvice
* what
  Controller를 보조하는 어노테이션으로 Controller에서 쓰이는 공통 기능들을 모듈화하여 전역으로 쓰기 위해 사용한다. `@ExceptionHandler`, `@InitBinder` , `@ModelAttribute`  세 가지의 어노테이션을 지원한다.

* when - how
1. 전역 예외 처리기로서 사용할 수 있다.
```
@ControllerAdvice
public class GlobalControllerAdvice {
    @ExceptionHandler(NullPointerException.class)
    public void nullPointerException(NullPointerException e) {
    }
}
```
애플리케이션 전역에서 발생한 특정한 예외를 @ControllerAdvice에서 잡아 처리할 수 있다.  예외처리에 대한 응답을 body로 전달하고 싶다면 @ResponseBody 어노테이션과 @ControllerAdvice를 합쳐 놓은 @RestControllerAdvice를 사용할 수 있다.


2. @ModelAttribute를 모든 컨트롤러에서 사용한다면 @ControllerAdvice에 선언하여 전역적으로 사용할 수 있다.
```
@RestControllerAdvice
public class GlobalControllerAdvice {
    @ModelAttribute
    public User user() {
        return new User("joanne");
    }
}
```
3. Validator, Formatter, Converter 등 여러가지 설정을 바인딩 할 수 있는 어노테이션인 @InitBinder를 ControllerAdvice에 작성하여 전역적으로 설정할 수 있다.

* why
  공통으로 수행되는 로직을 @ControllerAdvice 내부에서 전역적으로 처리하면 관리에 용이하다. 예를 들어 애플리케이션의 모든 컨트롤러에서 NullPointerException이 발생할 가능성이 있을 때, 각 컨트롤러에서 @ExceptionHandler 어노테이션을 붙인 메서드를 각각 작성해서 처리하는 것보다, @ControllerAdvice에서 전역적으로 처리하면 응답을 확인하기도 수월하다. 또한 예외를 비즈니스 로직에 넣지 않고 분리할 수 있다.
