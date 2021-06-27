---
layout: post
title: "[JPA] JPA 소개"
subtitle: "No more SQL based development.."
background: '/img/bg_technology.jpg'
categories: technology-javaandpython
---

### JPA Study의 시작

JPA는 **객체와 테이블의 Mapping**이다.

JPA를 사용하면 몇 십 줄의 SQL코드 및 DB관련 코드를 짧게 줄일 수 있으므로

개발생산성이 높아지고 유지보수가 용이해진다.

그러나 실무에서는 곧바로 적용하기가 어려운데,

실무에서는 객체와 테이블이 수십, 수백개가 있기 때문에 객체-테이블 관계의 **설계**가 어렵고,

JPA의 **내부 동작 방식**을 이해하지 못하기 때문이다.

나도 현재 토이프로젝트에서 JPA를 활용중인데,

사실 JPA에 대한 이해가 거의 없이 시작한 프로젝트라...

개발이 진행될수록 점점 개념 및 활용법에 대한 정리가 필요함을 느꼈다.

그래서 그 유명한 김영한님의 JPA 강의를 인프런에서 듣고 내용을 정리하기로 했다.

<br/>

#### SQL 중심 개발의 문제점

객체지향언어로 이루어진 벡엔드와 RDB는 서로 **객체**와 **테이블**로 구성되는데,

이 둘의 매핑이 되지 않기 때문에 **다수의 중복된, 복잡한 쿼리가 발생한다.**

즉, 개발이 **SQL 중심의 개발이 되어버린다(CRUD코드 무한반복).**

<br/>

가령, 기획자가 회원 정보를 만들어주라고 개발자한테 요구하여, 

개발자가 아래와 같은 객체 및 테이블(User)을 만들고 쿼리를 추가했다 하자.

```java
public User{
	String name;
	int age;
	String email;
    ...
}
```

```sql
INSERT INTO User(name, age, email) valies (.....)
SELECT name, age, email... FROM tb_user;
UPDATE tb_user SET (.....)
```

위와같은 상황에서 기획자가 **전화번호 컬럼도 추가해주세요!**라고 요구하면?

우리는 테이블의 컬럼을 하나 더 추가하고,

**그와 관련된 모든 쿼리문을 전부 수정해야만 한다.**

근데 만약 개발자가 Update 쿼리에 Set하는걸 깜빡하거나 하면? ....

이처럼 개발이 SQL에 의존적이므로, 효율이 떨어지고 개발 생산성이 저하되는 문제가 생긴다.

<br/>

#### 그래서 JPA란?

JPA 는 **Java application과 JDBC 사이**에서 동작한다.

개발자가 JPA에게 객체를 넘기면, JPA가 객체를 분석하여 쿼리를 생성하고,

JDBC API를 이용하여 DB에 명령을 내리는 역할까지 수행한다.

즉, **객체지향 Entity와 DB 테이블의 매핑을 지원**해주는 것이다.

JPA는 객체지향 언어의 **Collection Framework**가 객체를 다루는 방식과 유사하게 객체를 마법같이 다루며,

이로인해 **객체지향언어 - RDB 사이의 '패러다임 불일치'를 해결할 수 있도록 해준다.**

<br/>

#### JPA의 성능 최적화

JPA를 쓰면 SQL보다 성능이 더 나빠질까? 아니다.

JPA처럼 계층 사이에 중간 매개체를 쓰면 성능상 이점을 얻을 수 있는 부분이 있는데,

**버퍼**역할과 **캐싱** 기능을 활용할 수 있다. JPA에서도 마찬가지인데, 그로인한 이점은

1. **1차 캐시와 동일성(identity) 보장**
2. **트랜잭션을 지원하는 쓰기 지연 (transactiaonl write-behind)**
3. **지연 로딩 (Lazy loading)**

이다.

즉, JPA를 잘 쓰면 SQL을 쓰는 것 보다 오히려 성능을 더 끌어올릴 수 있다.

<br/>

#### 1차 캐시와 동일성(identity) 보장

JPA는 동일한 트랜잭션 안에서는 같은 엔티티의 반환을 보장한다(약간의 조회성능 향상).

```java
String memberId = "100";

Member m1 = jpa.find(Member.class, memberId); // SQL select 수행
Member m2 = jpa.find(Member.class, memberId); // 캐시

println(m1 == m2); // true를 반환

// 즉, select 쿼리가 한 번만 수행된다.
```

위 코드에서 find 메소드, 즉 select 쿼리는 캐싱 덕분에 **한 번만**수행되며,

따라서 **동일성(==)**을 보장할 수 있다.

<br/>

#### 트랜잭션을 지원하는 쓰기 지연 (Option 설정 가능)

매번 SQL을 날리면 그만큼 리소스가 낭비되며 네트워크를 타야한다.

그러나 JPA는 옵션을 통해 **버퍼링**기능을 지원하여 write를 지연시킬 수 있다. 

```java
transanction.begin(); // 트랜잭션 시작

em.persist(memberA);
em.persist(memberB);
em.persist(memberC);
// 여기까지 INSERT SQL을 DB에 보내지 않다가 (영속성 컨텍스트 버퍼링)

transaction.commit(); // 이 순간에 한꺼번에 커밋을 한다.
```

<br/>

#### 지연 로딩과 즉시 로딩 (Option 설정 가능)

**지연로딩**은 객체가 **실제 사용되는 순간**에 로딩하는 것이고,

**즉시로딩**은 JOIN SQL로 한 번에 연관된 객체까지 **미리 조회**를 하는 기능이다.

```java
Member member = memberDAO.find(memberId);

Team team = member.getTeam();

String teamName = team.getName();
```

위와 같은 코드가 있다고 할 때,

Member 객체와 Team 객체는 연관관계가 있으므로 member 인스턴스에서 getTeam() 으로

해당 member의 team을 알아낼 수 있다고 하자.

**지연로딩**전략은, member 인스턴스를 find 메소드로 select 할 때

**그와 연관된 team 객체의 정보는 가지고 오지 않고 있다가,**

team.getName(); 의 메소드가 실행될 때(실제 사용될 때) **team에 대한 select쿼리를 날려서**

team 정보를 가져오는 전략이다.

**즉시로딩**전략은, member 인스턴스를 find 메소드로 select 할 때

**그와 연관된 team 인스턴스의 정보도 JOIN 쿼리를 통해 미리 로딩하는 기능이다.**

즉, memberDAO.find(memberId) 메소드의 쿼리는

```sql
SELECT M.*, T.* FROM Member M JOIN Team T ...
```

위와 같은 형태가 될 것이다.

이 기능은 **옵션 설정**을 통해 언제든 변경할 수 있으며,

따라서 JPA는 상황에 맞는 최적화 전략을 개발자가 선택할 수 있게 제공해준다.

<br/>

이렇게 JPA는 다양한 전략과 최적화를 지원하기에,

JPA를 잘 다룰 줄 알면 개발 생산성 및 성능의 향상을 이끌어낼 수 있다.

다음 포스팅부터는 본격적으로 JPA의 개념과 사용법, 예제에 대해 기록해보겠다.

