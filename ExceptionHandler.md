### ExceptionHandler
* what
  @ExceptionHandler 뒤에 작성한 예외가 해당 컨트롤러에서 발생했을 때, 그 예외에 대한 전역적인 예외 처리를 할 수 있도록 하는 어노테이션
* when
  예외를 처리할 때, 각 예외가 발생하는 시점에서 try-catch로 처리하거나 밖으로 throw해서 처리해야한다. 이렇게 하는 경우, 중복되는 코드도 많아지고 관리해야하는 포인트도 많아지게 된다. 따라서 @ExceptionHandler 어노테이션을 이용해 특정 예외에 대한 처리를 공통으로 처리할 수 있도록 책임을 넘겨주고 싶을 때 사용한다.
* why
1. 특정 예외에 대한 처리를 한 곳에서 담당하도록 할 수 있다.
2. 특정 예외에 대한 응답이 어떻게 수행되는지 파악하기 쉽다.
3. 예외에 대한 관리가 용이하다. (특히 @ControllerAdvice와 함께 사용된다면 더욱 용이하다.)
* how
  특정 컨트롤러에서 발생하는 예외만 잡기 위해서는 해당 컨트롤러 내부에 메서드로서 작성할 수도 있고, 전역적으로 모든 컨트롤러에서 발생하는 특정 예외에 대한 처리를 하기 위해서는 @ControllerAdvice 내부 메서드로서 작성할 수 있다.

사용 예시는 다음과 같다.

```
@ExceptionHandler(DataException.class)
public ResponseEntity<ErrorResponse> passDataExceptionError(DataException e) {
    return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(new ErrorResponse(e.getMessage()));
} 
```
