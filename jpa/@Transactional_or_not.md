# 서비스에 @Transactional을 선언한 경우와 선언하지 않은 경우의 JPA save 동작

## 서비스에 트랜잭션이 달린 경우
```java
// postService.java
@Transactional
public void save() {
    repository.save(something);
}
```

```
2021-08-22 15:54:08.656 DEBUG 9192 --- [ http-nio-8080-exec-1 ] o.s.o.j.JpaTransactionManager : Creating new transaction with name [me.elastic.application.PostService.save]: PROPAGATION_REQUIRED,ISOLATION_DEFAULT
2021-08-22 15:54:08.657 DEBUG 9193 --- [ http-nio-8080-exec-1 ] o.h.e.t.i.TransactionImpl : On TransactionImpl creation, JpaCompliance#isJpaTransactionComplianceEnabled == false
2021-08-22 15:54:08.657 DEBUG 9193 --- [ http-nio-8080-exec-1 ] o.h.e.t.i.TransactionImpl : begin
2021-08-22 15:54:08.683 DEBUG 9219 --- [ http-nio-8080-exec-1 ] o.s.o.j.JpaTransactionManager : Exposing JPA transaction as JDBC [org.springframework.orm.jpa.vendor.HibernateJpaDialect$HibernateConnectionHandle@258a3970]
2021-08-22 15:54:08.688 DEBUG 9224 --- [ http-nio-8080-exec-1 ] o.s.o.j.JpaTransactionManager : Found thread-bound EntityManager [SessionImpl(1832642777<open>)] for JPA transaction
2021-08-22 15:54:08.688 DEBUG 9224 --- [ http-nio-8080-exec-1 ] o.s.o.j.JpaTransactionManager : Participating in existing transaction
```

1. 메소드가 시작되면 트랜잭션도 시작된다.
```
JpaTransactionManager : Creating new transaction with name [me.elastic.application.PostService.save]
```

2. JPA save()는 1에서 열린 트랜잭션에 참여한다.
```
JpaTransactionManager : Participating in existing transaction
```

## 트랜잭션을 선언하지 않은 경우
```java
// postService.java
public void save() {
    repository.save(something);
}
```

```
2021-08-22 15:57:57.316 DEBUG 4366 --- [ http-nio-8080-exec-1 ] o.s.o.j.JpaTransactionManager : Creating new transaction with name [org.springframework.data.jpa.repository.support.SimpleJpaRepository.save]: PROPAGATION_REQUIRED,ISOLATION_DEFAULT
2021-08-22 15:57:57.317 DEBUG 4367 --- [ http-nio-8080-exec-1 ] o.h.e.t.i.TransactionImpl : On TransactionImpl creation, JpaCompliance#isJpaTransactionComplianceEnabled == false
2021-08-22 15:57:57.317 DEBUG 4367 --- [ http-nio-8080-exec-1 ] o.h.e.t.i.TransactionImpl : begin
2021-08-22 15:57:57.346 DEBUG 4396 --- [ http-nio-8080-exec-1 ] o.s.o.j.JpaTransactionManager : Exposing JPA transaction as JDBC [org.springframework.orm.jpa.vendor.HibernateJpaDialect$HibernateConnectionHandle@7812fae6]
2021-08-22 15:57:57.356 DEBUG 4406 --- [ http-nio-8080-exec-1 ] o.h.e.s.ActionQueue : Executing identity-insert immediately
```

1. JPA의 save() 호출 시 JPA에 의해 트랜잭션이 시작된다.
```
2021-08-22 15:57:57.316 DEBUG 4366 --- [ http-nio-8080-exec-1 ] o.s.o.j.JpaTransactionManager : Creating new transaction with name [org.springframework.data.jpa.repository.support.SimpleJpaRepository.save]: PROPAGATION_REQUIRED,ISOLATION_DEFAULT
```

2. 메서드가 끝나면 commit도 JPA save()의 Transaction에서 수행한다.

> 결론! 스프링 데이터 JPA가 제공하는 Repository의 모든 메서드는 기본적으로 트랜잭션이 적용되어있음을 알 수 있다.

* rollback도 테스트해본 결과 @Transactional 어노테이션이 붙은 경우에만 예외 발생 시 롤백이 수행된다.


## 해당 로그를 보기 위한 logback.xml 설정 파일
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<configuration scan="true" scanPeriod="30 seconds">
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern> %d{yyyy-MM-dd HH:mm:ss.SSS} %highlight(%-5level) %magenta(%-4relative) --- [ %thread{10} ] %cyan(%logger{20}) : %msg%n </pattern>
        </encoder>
    </appender>
    <!-- 여기가 중요 -->
    <logger name="org.springframework.orm.jpa.JpaTransactionManager" level="DEBUG"/>
    <root level="debug">
        <appender-ref ref="CONSOLE"/> <!-- Console에 로그를 출력하고자 할 때 사용 -->
    </root>
</configuration>
```