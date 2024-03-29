## @Transactional
* what
  트랜잭션이란 데이터베이스의 상태를 변화시키기 위해 수행하는 작업의 단위를 뜻한다. 스프링에서는 트랜잭션 처리를 지원하는데, @Transactional 어노테이션을 통해 이를 수행할 수 있다.

* when
  다음과 같은 성질(ACID)을 만족시켜야하는 여러 가지 데이터베이스 작업이 수행되는 메서드에 대해 @Transactional 어노테이션을 달아 해결할 수 있다.
1. 원자성 - 한 트랜잭션 내에서 실행한 작업은 하나로 간주하므로 모두 성공하거나, 모두 실패이다.
2. 일관성 - 트랜잭션은 일관성 있는 데이터베이스 상태를 유지한다.
3. 격리성 - 동시에 실행되는 트랜잭션들이 서로 영향을 미치지 않도록 서로 격리해야한다.
4. 지속성 - 트랜잭션을 성공적으로 마치면 결과가 항상 저장되어야 한다.

* how
  클래스, 메서드 위에 @Transactional이 추가되면, 이 클래스나 메서드에 트랜잭션 기능이 적용된 프록시 객체가 생성된다. 이 프록시 객체는 @Transactional이 포함된 메소드가 호출될 경우 PlatformTransactionalManager를 사용하여 트랜잭션을 시작하고, 정상 여부에 따라 모두 커밋하거나 모두 롤백한다.

* why
  Transactional 어노테이션을 달지 않은 채 여러 데이터베이스에 접근하는 메서드를 수행하는 경우, 특정 메서드가 제대로 완료되지 않았을 때에도 다른 변수나 메서드의 상태가 변경될 수 있다. 따라서 한 메서드를 하나의 처리 단위로서 실행시키길 원할 경우 사용해야한다.

### 참고 자료
* [갓대희의 작은공간 :: Spring Transactional 정리 및 예제](https://goddaehee.tistory.com/167)
