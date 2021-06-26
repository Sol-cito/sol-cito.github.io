---
layout: post
title: "[JAVA JPA] JPA 소개"
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

다음 포스팅부터는 본격적으로 JPA의 개념과 사용법, 예제에 대해 기록해보겠다.

