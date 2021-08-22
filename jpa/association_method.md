# [Spring Data JPA] 연관관계 편의 메서드
## 연관관계 편의 메서드
양방향 연관관계를 맺을 때에는, 양쪽 모두 관계를 맺어주어야한다.
사실 JPA의 입장에서 보았을 때에는 외래키 관리자(연관관계의 주인)  쪽에만 관계를 맺어준다면 정상적으로 양 쪽 모두에서 조회가 가능하다.

```java
Team team1 = new Team("Team1", "민정팀");
em.persist(team1);

Member member1 = new Member("Member1","민정");
member1.setTeam(team1); // 연관관계 설정 member1 -> team1
```

하지만 객체까지 고려한다면, 양쪽 다 관계를 맺어야한다.
```java
Team team1 = new Team("Team1", "민정팀");
em.persist(team1);

Member member1 = new Member("Member1","민정");
member1.setTeam(team1); // 연관관계 설정 member1 -> team1
team1.getMembers().add(member1) // 연관관계 설정 tema1 -> member1
```

즉, 객체의 양방향 연관관계는 양쪽 모두 관계를 맺어주어야 순수한 객체 상태에서도 정상적으로 동작한다.

이렇듯 양방향 연관관계는 결국 양쪽 모두를 신경써야한다. 만약, setTeam과 getMembers().add를 각각 호출하면 실수가 발생할 수 있다. 따라서 양쪽 모두의 관계를 맺어주는 것을 하나의 코드처럼 사용하는 것이 안전하다.

```java
public void setTeam(Team team) {
    this.team = team;
    team.getMembers().add(this);
}
```

위와 같이 한번에 양방향 관계를 설정하는 메서드를 **연관관계 편의 메서드**라고 한다.

하지만 위와 같이 `setTeam` 메서드를 작성하는 경우 버그가 발생할 수 있다. 예를 들어서

```java
member1.setTeam(team1);
member1.setTeam(team2);
```
위와 같이 연속적으로 setTeam을 호출한 이후 team1에서 멤버를 조회하면 member1가 여전히 조회된다. team2로 변경할 때 team1과의 관계를 제거하지 않았기 때문이다.

```java
public void setTeam(Team team) {
    if (this.team != null) { // 기존에 이미 팀이 존재한다면
        this.team.getMembers().remove(this); // 관계를 끊는다.
    }
    this.team = team;
    team.getMembers().add(this);
}
```
따라서 위와 같이 기존 팀과의 관계를 제거하는 코드를 추가해야 정상적으로 동작한다.