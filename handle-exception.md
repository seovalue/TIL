## 예외를 처리하는 방법

* background
    * 사용자 정의 예외: 개발자가 직접 정의하여 사용하는 예외이다. 
    * checked exception: 개발자가 반드시 예외 처리를 직접 진행해야하는 예외이다.
    * unchecked exception: 개발자가 예외처리를 직접 하지 않아도 되는 예외이다. (명시적으로 예외 처리가 강제되지 않는 예외이다.) 
      `NullPointerException`, `ArrayIndexOutOfBoundException`, `NumberFormatException`등이 있다. 
    
    자바의 모든 예외 클래스는 java.lang.Exception 클래스를 상속받는데, Exception 클래스 자체는 checked Exception이며, 그 자식들 중 RuntimeException은 unchecked이다.
  

* 사용자 정의 예외는 언제 사용해야할까?  
    예외 메시지로도 충분히 의미를 전달할 수 있고, 표준 예외를 사용하면 가독성도 높아지기 때문에 표준 예외를 사용하는 것이 더 나을수도 있다.
  하지만 사용자 정의 예외를 사용하면 클래스 명으로도 정보 전달이 가능하며, 상세한 예외 정보를 제공할 수 있다. 또한 커스텀 예외를 사용하면 개발자가 인지한 상황에서 특정 예외를 던저주는 것이기 때문에 예외 발생에 대한 처리가 용이하다.
  

* 사용자 정의 예외는 어떻게 구성하는 것이 좋을까?
    예외 메시지 단위로 커스텀 예외를 생성하는 것은 수많은 예외 클래스를 낳고 관리가 어렵기에 매우 좋지 않은 방법이다. 
  따라서, 발생할 수 있는 가장 상위 행동으로 상위 예외 클래스를 만든 뒤, 도메인 별로 해당 상위 예외 클래스를 상속하여 구현한다. 그리고 각 예외 클래스는 예외 메시지만을
  가지며 HttpStatus는 ExceptionHandler에서 관리하도록 함으로서 상태 코드에 대한 응집도를 높인다. 더불어 예외 메시지에 상세 정보를 담을 수 있도록 구성함으로서 디버깅에 용이하도록 구현하는 것이 좋다고 생각한다.

### 참고 자료
- [Custom Exception을 언제 사용해야 할까?](https://woowacourse.github.io/javable/post/2020-08-17-custom-exception/)
- [리뷰어 휴의 의견](https://github.com/woowacourse/atdd-subway-path/pull/118#discussion_r633656164)