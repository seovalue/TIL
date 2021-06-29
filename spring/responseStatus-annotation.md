## @ResponseStatus

* what  
    반환되어야하는 상태 코드 및 이유를 예외 클래스 또는 메서드에 표시하는 용도로 사용되는 어노테이션이다.  
  핸들러 메소드가 호출될 때 상태 코드가 HTTP response에 적용되고 ResponseEntity 또는 redirect와 같은 다른 수단으로 설정된 상태 정보를 대체한다.
  
* 주의할 점  
  > Warning: when using this annotation on an exception class, or when setting the reason attribute of this annotation, the HttpServletResponse.sendError method will be used.
  With HttpServletResponse.sendError, the response is considered complete and should not be written to any further. Furthermore, the Servlet container will typically write an HTML error page therefore making the use of a reason unsuitable for REST APIs. For such cases it is preferable to use a org.springframework.http.ResponseEntity as a return type and avoid the use of @ResponseStatus altogether.
  
    예외 코드 및 이유를 반환할 때 사용할 수 있지만, 이 `@ResponseStatus`를 사용할 경우 HttpServletResponse.sendError() 메서드가 사용된다.
  이 메서드가 사용되면 응답이 완료된 것으로 간주되며 서블릿 컨테이너는 일반적으로 HTML 오류 페이지를 작성하므로 REST API의 예외 응답에는 적절하지 않다. 따라서 REST API의 예외 응답으로는 org.springframework.http.ResponseEntity를 반환 유형으로 사용하고 @ResponseStatus를 사용하지 않는 것이 좋다. 
  
  
  만약 @ResponseStatus를 이용해 상태 코드를 지정하고, 응답으로는 예외 객체를 뱉는다면 `@RestControllerAdvice`와 함께 다음과 같이 사용은 할 수 있되, 
ResponseEntity에 상태 코드를 싣어서 보내는 것을 권장한다.
  ```java
      @ResponseStatus(HttpStatus.BAD_REQUEST)
      @ExceptionHandler(NoExistException.class)
      public ErrorResponse handleNotFoundException(NoExistException e) {
          return new ErrorResponse(e.getMessage());
      }
      
      // 권장
      @ExceptionHandler(NoExistException.class)
      public ResponseEntity<ErrorResponse> handleNotFoundException(NoExistException e) {
            return ResponseEntity.badRequest().body(new ErrorResponse(e.getMessage()));
      }
  ```

### 참고 자료
- https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ResponseStatus.html