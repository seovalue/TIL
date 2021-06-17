Layered Architecture
* what
  layered architecture란 유사한 관심사들을 레이어로 구분하여 추상화한 뒤 수직적으로 배열하는 아키텍처이다. 주로 3계층 구조로 나뉘어 구분되는데, 3계층 구조에는 Data, Application, Presentation으로 구분된다.

    * Presentation Layer: 응용 프로그램의 최상위 계층에 위치하며 다른 층의 데이터와 소통한다. 사용자 인터페이스를 제공한다.
    * Application Layer: 비즈니스 로직 또는 트랜잭션 계층으로 불린다. 클라이언트의 요청에 대해 비즈니스 로직을 수행하는 서버처럼 행동하며, Data Layer에 대해서는 클라이언트처럼 행동하는 Middleware 역할을 한다.
    * Data Layer: 데이터베이스와의 상호작용을 수행하는 것을 관리하는 프로그램을 포함하는 계층이다.

[image:FDDFFB43-AD84-444B-881E-892625AAE039-4052-000001E51CFC58E9/95432828-1fe26680-098a-11eb-8a90-00f4d3342fc0.png]

* when
  시스템을 레이어로 나누면 시스템 전체를 수정하지 않고도 특정 레이어를 수정 및 개선할 수 있기에 사용한다.
  
  1. 단방향 의존성: 각 레이어는 하위 인접한 레이어에게만 의존
  2. 관심사의 분리: 각 레이어는 주어진 고유한 역할만을 수행
  3. 코드의 확장성과 재사용성: 각 레이어가 서로 독립적이며 역할이 분명하기에 서로에게 끼치는 영향을 최소화하여 수정 가능 / 독립 레이어이므로 하나의 서비스 레이어는 여러개의 프레젠테이션 레이어가 재사용할 수 있다.
  4. 코드의 가독성
  

* more
  * Presentation Layer: Web Layer이자 UI Layer이다. 화면 조작 또는 사용자의 입력을 처리하기 위한 관심사를 모아 놓은 계층이라고 볼 수 있다.
    즉, 최종 사용자에게 UI를 제공하거나 클라이언트로 응답을 다시 보내는 역할을 담당하는 모든 클래스이다.
    API의 엔드포인트를 정의하고 전송된 요청들을 읽어 들이는 로직을 포함한다.
    
  * Application Layer: Service Layer와 유사하다. 일반적으로 다양한 도메인과 비즈니스 로직을 의미있는 수준으로 묶어 제공하는 역할을 수행하는 계층이다.
    핵심 비즈니스 로직을 직접 구현하기보다는 도메인에게 적절한 책임을 분배하며 얕은 비즈니스 로직을 구성한다. Application Layer에서는 도메인 모델 및 Presentation Layer 사이에서
     도메인 기능을 응용하는 로직을 담당하며, Service Layer에서는 트랜잭션 및 도메인 모델의 기능간의 순서만을 보장해야한다.
    또한 도메인 모델을 직접적으로 반환하지 않고, DTO등을 반환하도록 함으로서 도메인 모델을 캡슐화하는 역할을 수행할 수 있다.
    
  * Data Layer: DataSource를 직접 활용하며 DB에서 데이터 저장, 수정 등의 DB와 관련된 로직을 수행하는 계층이다.
  
* why
  Layered Architecture에 대해 학습해본 이유는, 이번에 Service가 Service를 가져도 되는가?라는 의문에 대해 리뷰어이신 데이브께서 레이어드 아키텍쳐에 경우에는 
  서비스가 서비스를 가지는 경우도 존재한다고 하셔서 학습의 동기가 되었다. 레이어드 아키텍쳐는 앞서 말했듯 시스템을 관심사로 구분하여 수직적으로 쌓았기 때문에 여러개의 서비스가 존재할 때, 
  그리고 그 서비스들이 수직적 관계(말이 이상한데.. 단방향 관계가 확실한 경우를 말하고 싶었다.)를 갖는 경우에는 서비스가 서비스를 가져도 된다는 의견을 확립할 수 있었다.

### 참고 자료
* [Spring과 Layered Architecture | Chasing Yesterday](https://xlffm3.github.io/spring%20&%20spring%20boot/LayeredArchitecture/)
* [이해하기 3계층 구조 (3 Tier Architecture) | STEVEN J. LEE](https://www.stevenjlee.net/2020/05/08/%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-3%EA%B3%84%EC%B8%B5-%EA%B5%AC%EC%A1%B0-3-tier-architecture/)