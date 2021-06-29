## DTO는 어느 레이어까지 사용하는 것이 맞을까?
* background
  
  - DTO란? 계층 간 데이터 교환을 위한 객체이다. DTO는 단순히 전달만을 위한 객체이므로 비즈니스 로직을 내부에 갖지 않는다. 
  - MVC 패턴이란? 애플리케이션을 개발할 때 그 구성 요소를 Model, View 및 Controller 등 세 가지 역할로 구분하는 디자인 패턴이다. 
  비즈니스 처리 로직(모델)과 view영역은 서로의 존재를 인지하지 못하고, Controller가 중간에서 Model과 View의 소통을 담당한다. 이 때, 모델에서 넘겨받은 데이터를 뷰에게
    전달하기 위해 DTO를 사용할 수 있다.
* how
  
  DTO 사용 예시
  ```java
  // Station 도메인
  public class Station {
    private final long id; //외부에 노출되면 안되는 정보
    private final String name;
    
    public Station(long id, String name) {
        this.id = id;
        this.name = name;
    }
  }
  
  // StationDto
  public class StationDto {
    private final String name;
    
    public StationDto(String name) {
        this.name = name;
    }
  } 
  ```
* DTO는 어느 범위까지 사용해야할까?

  `DTO로의 변환은 컨트롤러에서 하는게 맞지 않을까요?`,`서비스에 DTO가 들어가는 것이 맞을까요, 도메인으로 변환한 뒤 들어가는 것이 맞을까요?`,
  `DTO가 dao까지 들어가고 있어요😱` 등의 의견이 이 질문에 대해 생각해보게 된 계기가 되었다.
  
  별다른 이유 없이 나는 DTO가 dao까지 들어가는 것은 물론 안되는 것이 맞으며, 컨트롤러로 가기 전 Service에서 비즈니스 로직 수행 후 DTO로 변환한 뒤 전달해주어야한다는 생각을 갖고 있었다.
  그리고, 컨트롤러에서 서비스를 호출할 때에도 DTO를 전달하는 것은 별다른 문제가 되지 않는다고 생각했다. 
  
  ![mvc layer](https://user-images.githubusercontent.com/56240505/95434101-d4c95300-098b-11eb-9d03-e6d3a586ebaa.png)

  우선, dao에 dto가 들어가면 안된다는 생각을 갖게 해줬던 건 위 이미지이다. 위 이미지는 Layered Architecture를 설명할 때 구조를 나타내는 이미지로서 스프링 부트와 aws로 혼자 구현하는 웹 서비스라는 책에서 처음 접하게 되었다.
  이 이미지에서도 볼 수 있듯이 dto는 Web Layer와 Service Layer간의 데이터 전송을 위해 사용되고, Service와 DAO는 도메인으로 소통한다. 
  Repository Layer에 dto가 들어가면 안되는 이유로는, repository layer에서는 엔티티의 영속성을 관장하는 역할이기 때문에 도메인의 dto 변환 및 dto에서 도메인의 변환을 repository layer에서 책임지게 하는 것을 지양하자는 의견이 존재한다는 것도 하나의 이유가 된다.
  
  그럼 다음으로, DTO의 변환은 그럼 서비스에서 하는 것이 맞을까?
  > 한줄 요약: 서비스는 도메인을 보호하는 역할을 가지기 때문에, 컨트롤러로 결과를 전달할 때에도 한번 캡슐화한 뒤(dto로 변환) 전달해야한다.
  
  우선, Service Layer는 어플리케이션의 경계를 정의하고 비즈니스 로직 등 도메인을 캡슐화하는 역할이라고 `마틴 파울러`가 말했다.
  즉, Service Layer는 도메인의 캡슐화를 수행하는 책임을 가지기 때문에, Controller로 결과를 전달할 때에도 DTO로 변환해서 전달하는 것이 도메인을 더 보호하는 결과라는 생각이 들었다.
  또한 도메인이 컨트롤러까지 넘어가게 된다면, 도메인의 변경이 있을 시 컨트롤러까지 그 영향이 미치기 때문에 유지보수에도 좋지 않다. 
  위의 이유들로 보자면 DTO의 변환은 서비스 레이어에서 수행하는 것이 맞는 것 같다.
  
  그렇다면, 컨트롤러에서 DTO로의 변환은 수행하면 안되는 것인가?
  
  먼저 컨트롤러에서 서비스로의 dto 진입에 대해서부터 생각해보자.
  > 한줄 요약: 서비스로 dto의 진입을 허용하지 않게 되면, 컨트롤러에서 dto 변환을 수행해야한다.
  
  앞선 layered architecture에서는 layer별로 역할이 정해져 있기 때문에, 각 역할이 제대로 구분되어 있다면 Presentation Layer가 여러 개의 Application Layer를 이용할 수 있다고 되어 있다. 즉, 레이어드 아키텍쳐로 구성한다는 것 자체가 각 레이어의 책임을 명확히 분리한다는 의미이다.
  그렇다면, DTO가 service로 들어가게 될 시 Service는 해당 dto를 사용하는 컨트롤러에 종속적이게 된다. 그리고, dto가 한번 서비스까지 들어가게 되면 dao까지 진입하게 되는 결과를 낳을 수 있다.
  그럼, 이에 대한 결론을 내리면 서비스는 dto를 모른 채 도메인만을 가지고 설계하는 것이 맞고, 서비스는 dto를 모르기 때문에 컨트롤러로 결과를 전달할 때에도 도메인으로 전달해야한다.
  즉, 도메인을 dto로 변환하는 작업 또한 컨트롤러에서 수행해야하는 것이다.
  
* Conclusion
  결론을 내려보자면, 각 레이어의 책임이 어떻게 구분하는지, 그리고 Entity가 어느 계층까지 노출되어도 가능하냐 등의 종합적인 이유를 따져본 뒤 어느 계층에서 dto 변환을 수행해야할 지 결정해야한다.
  하지만 나라면, `컨트롤러로 도메인이 노출되는 경우 변경의 범위가 크다.`는 이유가 가장 와닿았기 때문에 Service에서 컨트롤러로 데이터를 전달할 때 dto로 변환한 뒤 전달을 수행할 것 같다.

### 참고 자료
* [DTO는 어느 레이어까지 사용하는 것이 맞을까? :: SLiPP](https://www.slipp.net/questions/93)
* [Tecoble DTO의 사용 범위에 대하여 | Chasing Yesterday](https://xlffm3.github.io/spring%20&%20spring%20boot/DTOLayer/)
* [DTO의 적절한 사용에 대한 고민](https://velog.io/@maigumi/DTO%EC%9D%98-%EC%A0%81%EC%A0%88%ED%95%9C-%EC%82%AC%EC%9A%A9%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B3%A0%EB%AF%BC)