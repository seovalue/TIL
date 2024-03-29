# [JPA] 영속성 관리

> 이 글은 모두 [자바 ORM 표준 JPA 프로그래밍](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788960777330) 도서를 통해 학습하고 정리한 내용입니다.

## 엔티티 매니저 팩토리와 엔티티 매니저
* 엔티티 매니저
  엔티티를 저장하고, 수정하고, 삭제하고, 조회하는 등 엔티티와 관련된 모든 일을 처리한다. (엔티티를 저장하는 가상의 데이터베이스라고 생각하면 이해하기 쉽다.)

데이터베이스 연결이 꼭 필요한 시점까지 커넥션을 얻지 않는다. 즉, 트랜잭션을 시작할 때 커넥션을 획득한다.

* 엔티티 매니저 팩토리
```java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("book");
```

엔티티 매니저 팩토리(이하 엔매팩)는 엔티티 매니저를 만드는 공장이다. 공장을 만드는 비용은 매우 커서 애플리케이션 당 한개만 만들어서 공유하도록 설계되어있다. 엔매팩은 여러 스레드가 동시에 접근해도 안전하므로 서로 다른 스레드 간에 공유해도 되지만, 엔티티 매니저는 여러 스레드가 동시에 접근하면 동시성 문제가 발생하므로 스레드 간에 공유가 불가능하다.

## 영속성 컨텍스트란?
* 영속성 컨텍스트
  엔티티를 영구 저장하는 환경이라는 뜻이다. 엔티티 매니저로 엔티티를 저장하거나 조회하면 엔티티 매니저는 영속성 컨텍스트에 엔티티를 보관하고 관리한다.

예를 들어, `em.persist(member);` 는 엔티티 매니저를 사용해서 회원 엔티티를 영속성 컨텍스트에 저장한다는 의미를 가진다.


## 엔티티의 생명주기
* 비영속(new/transient): 영속성 컨텍스트와 전혀 관계가 없는 상태
* 영속(managed): 영속성 컨텍스트에 저장된 상태
* 준영속(detached): 영속성 컨텍스트에 저장되었다가 분리된 상태
* 삭제(removed): 삭제된 상태

![엔티티의 생명주기](https://ultrakain.gitbooks.io/jpa/content/chapter3/images/JPA_3_2.png)

### 비영속
엔티티 객체를 생성하고 아직 저장하지 않았을 때

```java
Member member = new Member("조앤");
```

### 영속
엔티티 매니저를 통해서 엔티티를 영속성 컨텍스트에 저장한 상태. 영속성 컨텍스트가 관리하는 엔티티를 **영속 상태**라고 한다.

```java
em.persist(member);
```

### 준영속
영속성 컨텍스트가 관리하던 영속 상태의 엔티티를 영속성 컨텍스트가 관리하지 **않으면** 준영속.

```java
em.detach(member);
em.close(); // 아예 영속성 컨텍스트를 닫아버린다.
em.clear(); // 영속성 컨텍스트를 아예 초기화해버린다.
```

### 삭제
엔티티를 영속성 컨텍스트와 데이터베이스에서 삭제한 상태

```java
em.remove(member);
```

## 영속성 컨텍스트의 특징
1. 식별자 값으로 엔티티를 구분한다. 따라서 영속 상태는 식별자 값이 반드시 있어야한다.
2. JPA는 트랜잭션을 커밋하는 순간 영속성 컨텍스트에 새로 저장된 엔티티를 DB에 반영한다. 이를 `flush`라고 한다.
3. 1차 캐시, 동일성 보장, 쓰기 지연, 변경 감지, 지연 로딩

### 엔티티 조회
영속성 컨텍스트 내부에는 캐시가 하나 존재한다. 이를 우리는 1차 캐시라고 부른다.
1차 캐시는 키는 @Id로 매핑한 식별자, 값은 엔티티 인스턴스를 갖는 Map 형태라고 생각하면 이해하기 쉽다.

우리가 회원을 생성하고, 엔티티 매니저를 통해 persist를 수행하면 이 엔티티는 **아직 데이터베이스에 저장되지 않는다.**
앞서 1차 캐시의 키는 식별자 값이라고 했었다. 이 키는 데이터베이스의 기본키 값과 매핑된다. 그래서 우리는 엔티티를 조회할 때 다음과 같이 조회할 수 있다.

`em.find(Member.class, “member1”);`
1번째 파라미터는 엔티티 클래스의 타입, 2번째 파라미터는 엔티티의 식별자 값이다. 엔티티 매니저를 통해 조회를 수행할 때,  먼저 영속성 컨텍스트 내부의 1차 캐시에서 엔티티를 찾고, 만약 찾는 엔티티가 1차 캐시에 없으면 데이터베이스에서 해당 식별자 값을 바탕으로 조회한다.

> 만약 `em.find()`의 결과가 1차 캐시에 없다면?  
앞서 데이터베이스에서 조회하여 엔티티를 생성한다고 했다. 이 때, 신기하게도 DB에서 조회한 뒤 조회된 엔티티를 1차 캐시에 저장하고, 저장된 영속 상태의 엔티티를 반환한다.

즉, 다시 조회했을 때 1차 캐시에서 가져올 수 있으므로 성능상 이점을 가져올 수 있다.

* 여기서 더 나아가 조회한 엔티티의 동일성과 동등성에 대해서 고민해볼 수 있다.
  우리가 엔티티 매니저를 이용해 find를 수행한다면, 엔티티 매니저는 항상 1차 캐시에 있는 같은 엔티티 인스턴스를 반환하기 때문에 항상 같은 인스턴스를 가져오고, 동일성을 보장한다.

```
동일성: 실제 인스턴스가 같다. 참조 값을 비교하는 ==이 같다.
동등성: 실제 인스턴스는 다를 수 있지만, 인스턴스가 가진 값이 같다. eqauls() 메서드를 이용해 비교할 수 있다.
```


### 엔티티 등록
엔티티 매니저를 사용해서 엔티티를 영속성 컨텍스트에 등록하는 것까지는 위 예제들을 통해 확인할 수 있었다. 그럼, 데이터베이스에 Insert SQL은 어떻게 보낼 수 있을까?

엔티티 매니저는 데이터 변경 시 트랜잭션을 시작해야한다. 시작된 트랜잭션 내부에서 em.persist가 수행되고, 마지막에 `transaction.commit()` 이 수행됨으로서 드디어 INSERT SQL을 데이터베이스에 전송한다.

즉, 엔티티 매니저는 트랜잭션을 커밋하기 전까지 DB에 엔티티를 저장하지 않고, 내부 쿼리 저장소에 INSERT SQL을 모아둔다. 그리고 트랜잭션을 커밋할 때 모아둔 쿼리를 보낸다. 이를 **쓰기 지연(transactional writed-behind)** 라고 한다.

어떻게 이 방법이 가능한 것일까?
```
begin();

save(A);
save(B);

commit();
```

A. 데이터를 save 할 때 즉시 삽입 쿼리를 디비에 보낸다. 그리고 마지막에 트랜잭션을 커밋한다.
B. 데이터를 저장하면 등록 쿼리를 디비가 아닌 메모리에 모아둔다. 그리고 트랜잭션을 커밋할 때 모아둔 등록 쿼리를 디비에 보내고 커밋한다.

결과적으로 트랜잭션의 범위 내에서 실행되므로 A와 B의 결과는 같다. 즉, A처럼 그때 그때 데이터베이스에 전달해도, 트랜잭션을 커밋하지 않으면 아무 소용이 없기 때문에 어쨌던 트랜잭션 커밋 직전에만 데이터베이스에 SQL을 전달하기만 하면 된다. 그래서, 트랜잭션을 지원하는 쓰기 지연이 가능하다.

### 변경 감지
JPA로 엔티티를 수정할 때는 단순히 엔티티를 조회해서 데이터만 변경하면 된다. 이러한 기능을 **변경 감지 (dirty checking)**이라고 한다.

엔티티를 영속성 컨텍스트에 보관할 때, 최초 상태를 복사해서 저장해두는데 이를 스냅샷이라고 한다. 그리고 flush 시점에 스냅샷과 엔티티를 비교해서 변경된 엔티티를 찾는다. 변경된 엔티티가 존재한다면 쓰기 지연 SQL 저장소에 수정 쿼리를 보내고, 트랜잭션을 커밋한다.

* 변경 감지는 영속 상태의 엔티티에만 적용된다.
* 변경 감지 시 엔티티의 모든 필드를 업데이트한다.
    * 필드가 많거나 저장되는 내용이 클 시에는 `@org.hibernate.annotations.DynamicUpdate` 어노테이션을 사용해 수정된 데이터만 사용해서 동적으로 Update SQL을 생성할 수 있다.
    * 데이터가 존재하는 필드만을 삽입하는 `@DynamicInsert`도 존재한다.


## 플러시
플러시는 영속성 컨텍스트의 변경 내용을 데이터베이스에 반영하는 것을 말한다.

**flush를 수행하는 방법**
1. em.flush()
2. 트랜잭션 커밋 시 자동으로 플러시
3. JPQL 쿼리 실행 시 플러시 자동 실행

### 플러시 모드
엔티티 매니저에 플러시 모드를 직접 지정하기 위해서는 `javax.persistence.FlushModeType`을 사용하면 된다.

```java
entityManager.setFlushMode(FlushModeType.COMMIT);
```

* FlushModeType.AUTO: 커밋이나 쿼리를 실행할 때 플러시 (default)
* FlushModeType.COMMIT: 커밋할 때만 플러시

## 준영속
영속성 컨텍스트가 관리하는 엔티티가 detach된 상태를 준영속 상태라고 한다.

* detach
  detach 메서드를 호출하면 1차 캐시부터 쓰기 지연 SQL 저장소까지 해당 엔티티를 관리하기 위한 모든 정보가 제거된다.

이 외에도 clear, close 등이 있다.

### 병합 (merge)
JpaRepository에서 제공하는 save 메서드를 실행하면, 내부적으로 엔티티가 isNew가 아닌 경우 `em.merge()` 를 수행한다. merge는 무엇일까?

merge를 호출하면, 준영속/비영속 상태의 엔티티를 다시 새로운 영속 상태의 엔티티를 반환한다.
