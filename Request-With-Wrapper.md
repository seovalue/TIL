## Request에는 의식적으로 Wrapper 타입을?

Request의 field에는 의식적으로 Wrapper 타입을 사용해주어야하는 것일까?

## Primitive Type (기본형)

우선, 비객체 타입으로 `Null`을 가질 수 없는 형태를 갖는다. `null`이 들어오는 경우 `NullPointerException`을 발생시킨다.
또한 아래와 같이 default value를 갖는다.

|Data Type|Default Value (for fields)|
|--|--|
|byte| 0|
|short| 0|
|int| 0|
|long|0L|
|float|0.0f|
|double| 0.0d|
|char|'\u0000'|
|String (or any object)|null|
|boolean|false|


## Wrapper Class
래퍼클래스는 기본형을 객체로 한번 감싼 클래스이다. 따라서 기본형을 객체로 다룰 수 있도록 해준다. 더불어 Wrappper 클래스는 null을 가지지 못하던 기본형을 객체로 한번 감쌌기 때문에 null을 가질 수 있다. 그리고 래퍼클래스는 기본형과 다르게 default 값을 null로 가진다. 
기본 타입을 래퍼 클래스로 감싸는 과정을 `Boxing`,  래퍼 클래스로부터 기본 타입을 얻어내는 과정을 `Unboxing`이라고 한다. Java에서는 래퍼 클래스와 기본 타입을 혼용하며 사용할 때, 자동으로 박싱과 언박싱이 일어나게 된다. 예를 들어, int 타입의 값을 Integer 변수에 대입하면 자동 박싱이 일어나 힙 영역에 Integer 객체가 생성된다.
여기서 주의해야할 점은 Wrapper 클래스에서는 null을 허용하지만, 기본 타입에서는 허용하지 않기 때문에 언박싱 과정에서 NPE가 발생할 수 있으므로 주의해야한다.

* When to Use Wrapper Class and Primitive Type in Request DTO  
    - Wrapper Class를 사용했을 때의 장점  
        원하는 값이 들어왔을 때에만 그 값을 할당할 수 있다. (default가 null이므로)  
        null이 들어왔을 때 NPE를 발생시키지 않으므로 로직 상 null이 필요한 경우에 사용할 수 있다.
    - Primitive Type을 사용했을 때의 장점
        기본 타입이 성능 상 더 낫다.
        해당 필드를 생략해도 default가 들어가므로 null이 할당되지 않아 예기치 못한 NPE를 줄일 수 있다.
    - 결론  
        우선 이 글의 가장 큰 주제는 Request DTO에서의 wrapper class와 primitive class이므로, 우선 토대로 생각해보았을 때 ID와 같은 값에는 우선 null을 할당하고, DB에 
      데이터를 넣은 뒤 값을 할당하기 때문에 이런 경우에는 Wrapper를 사용하는 것이 맞다고 생각된다. (0L을 할당하면 실제 0L의 id를 갖는 값이 존재할 수도 있다.) 또한 기본 타입에서 저절로
       할당되는 default value를 절대로 허용하지 못하는 경우 또한 Wrapper를 사용하는 것이 맞을 것이다. (ex. 0으로 초기화되는 경우 비즈니스 로직에 문제를 일으킬 때) 
      이 외의 경우에는 성능 및 npe에서 생각해보았을 때 기본 타입을 사용하는 것이 더욱 우선순위일 것이라 생각된다.

### 참고 자료
* https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html
* https://stackoverflow.com/questions/1570416/when-to-use-wrapper-class-and-primitive-type/1570470