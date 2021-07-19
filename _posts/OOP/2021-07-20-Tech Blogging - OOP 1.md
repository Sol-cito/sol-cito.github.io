---
layout: post
title: "[OOP] OOP 바로알기 (1)"
subtitle: "객체는 '기능'으로 정의된다."
background: '/img/bg_technology.jpg'
categories: technology-os

---

### 객체지향의 의미

객체지향적으로 코드를 작성해야 하는 이유는, **그렇지 않으면 Product 생산시간이 계속 늘어나기 때문이다.**

즉, **유지보수 비용이 계속 증가한다**는 것인데,

코드가 방대하고 지저분해질수록 **코드 분석 시간**이 증가하며, **코드 변경 시간**또한 증가한다.

Software Product의 가치는 **'동작 가능함'**이 아니라, **'지속 가능한 변화'**에 있다. 

현실 세계는 계속 변화하고, 그에 따라 **요구사항** 또한 계속 변화하기 때문이다.

> Software maintenance is not to keep it working like before, 
>
> but to keep being useful in a changing world.

시간이 조금 지나고, product가 낡아서 ( = 유지보수가 불가능할 정도로 지저분해져서),

매번 차세대 프로젝트를 통해 비용을 투자하게 된다면, 이는 비즈니스관점에서도 비효율적일수밖에 없다.

**변화에 유연한 설계를 통해 변화에 adaptable하고 sustainable한 Product를 만드는 것,**

그리고 그 전략 중 하나가 바로 **객체지향**이다.

---

### 객체지향의 특징 - '기능'으로 정의되는 것.

객체는 특정 객체가 가진 **내부 데이터(필드)**가 아니라, **그것이 가진 '기능'**으로 분류된다.

그리고 그 기능의 '명세'는 **메서드**로 제공된다.

객체와 객체는 **기능**을 사용하여 연결되는데, 즉 **다른 객체의 메소드를 호출하여 해당 객체의 기능을 사용하는 것이다.**

이렇게 객체와 객체가 기능을 주고받으면서 상호작용하는 것을 **메시지를 주고받는다**고 표현한다.

메소드를 호출하거나, 결과를 return하거나, Exception을 주거나 하는 객체들 사이의 행위들을 **메시지**라고 부른다.

자 그럼, 아래 클래스는 객체라고 할 수 있을까?

```java
public class Member(){
    private String name;
    
    public Member(String name){
        this.name = name;
    }
    
    public String getName(){
        return name;
    }
    
    public void setName(String name){
        this.name = name;
    }
}
```

위 클래스는 사실상 **기능이 없다**.

setter와 getter는 해당 클래스의 기능이라기보다는, **필드 데이터의 제공 및 조작**이 목적이다.

따라서 위 클래스는 **구조체 or 데이터 클래스** 등으로 표현된다.

<br/>

---

### 캡슐화(Encapsulation)

캡슐화는 **객체가 기능을 어떻게 구현했는지 외부가 알 수 없도록 만드는 것**이다.

즉 **구현에 사용된 Data의 상세 내용을 감추는 것(정보 은닉)**이다.

이게 무슨 의미이며, 왜 Data의 내용을 감추어야 할까?

<br/>

회원 클래스(Member)의 멤버십 종류에 따라 선물이 달라지는 기능을 구현한다고 했을 때,

데이터를 감추지 않은(캡슐화를 하지 않은) 아래 코드를 보자.

```java
Member member = new Member(); // 회원 클래스

(...)

if(member.getMembershipKind == "Gold"){
    // 선물을 주는 코드
}
```

Gold 멤버에게만 선물을 주는 코드를 구현하였다.

그런데...몇 달이 지나 선물을 Gold 멤버 뿐 아니라 Silver, Bronze 에게도 주어야 하는 상황이 된다면?

위 코드는 아래처럼 바뀔 수 밖에 없을 것이다.

```java
Member member = new Member(); 

(...)

if(member.getMembershipKind == "Gold" || member.getMembershipKind == "Silver" 
   || member.getMembershipKind == "Bronze"){
    // 선물을 주는 코드
}
```

딱 봐도 코드량이 늘어나고 지저분해진다.

게다가, 

**해당 로직이 있는 부분이 한 군데가 아니라 수십 군데라면?**

**멤버십 종류가 늘어나고 명칭이 변경된다면?**

**멤버십 뿐만 아니라 다른 조건을 고려해서 선물을 주는 요구사항이 들어온다면?**

요구사항에 맞는 데이터는 잦은 변경을 수반하므로, 

**데이터를 직접 조작하면 그만큼 유지보수 비용이 늘어날 수밖에 없다.**

아래 코드를 보자.

```java
Member member = new Member(); 

(...)

if(member.isIncludedForEvent){
    // 선물을 주는 코드
}

// 회원 클래스
class Member{
    // feild 데이터는 private으로 감춘다.
    
    public boolean isIncludedForEvent(){
        if(특정 조건){
            return true;
        }
        return false;
    }
}
```

위와 같이 Member 클래스가 이벤트에 회원이 포함되어야 하는지 아닌지를 판단하는 메소드를 지닌다면,

**변경사항이 발생하더라도 개발자는 해당 메소드만 수정하면 된다.**

**왜냐하면 다른 모든 부분에서 데이터의 직접 조작이 아니라 isIncludedForEvent() 메소드를 사용하고 있을 것이기 때문이다.**

즉, 위 Member 클래스는 **필드 변수(데이터)**를 감추고(은닉), 

isIncludedForEvent 메소드 또한 그 구현 내용을 **외부에 보여주지 않는다.**

위 전략은 **캡슐화된 기능을 사용하는 코드의 영향을 최소화**할 수 있다.

<br/>

이 전략을 **Tell, Don't ask** 라고 한다**(Don't ask but tell me** 가 더 적당하지 않을까 싶긴 한데...).

즉, **다른 객체에게 데이터를 묻지 말고(Don't ask), **

**그 객체에게 내가 원하는 기능(ex)현재 그 멤버가 이벤트 대상인지 확인하는 기능)을 수행하여 결과를 말해달라고(Tell)** 하는 것이다.

<br/>

또 다른 캡슐화 전략은 **Demeter's Law(데메테르의 법칙)**이다.

데메테르의 법칙은 **객체의 메서드 호출의 결과(data)를 가지고 다른 메서드를 연속적으로 호출하지 말라**는 법칙으로,

즉 **메서드에서 생성했거나, 파라미터로 받았거나, 필드로 참조하는 객체의 메서드만 호출**하라는 법칙이다.

아래 코드를 보면,

```java
Date date = member.getRegistrationDate();
boolean flag = date.isGoldMember();
```

Date 라는 데이터를 얻기 위해 member에서 getRegistrationDate를 호출하고(즉, 필드변수를 건드리고 있다),

그 결과를 가지고 다시 GoldMember인지 아닌지를 판단하는 메소드를 호출한다.

이렇게 코드를 짜면, 나중에 요구사항이

**Date가 아니라 다른 기준으로 Gold멤버인지 아닌지를 판단해야 하거나,**

**가입일(RegistrationDate)이 아닌 다른 기준으로 날짜를 계산하거나** 처럼 변경되는 경우,

유지 보수 비용이 늘어날 것이 자명하다.

따라서 위와 같은 경우도,

```java
boolean flag = member.isGoldMember();
```

와 같이 Member 클래스에 **Gold멤버인이 아닌지 알려주는 '기능'**을 넣어주면,

객체지향 및 캡슐화가 가진 장점을 백분 활용할 수 있다.

<br/>

이렇게 생각하자.

Class A의 '기능'이 있을 때, 해당 기능은 Product의 **다양한 로직에서 사용**된다.

따라서,  해당 로직을 하나하나 전부 바꾸는 것 보다는,

**공통 부분인 Class A 메소드 자체의 수정**을 통해 **유지보수의 효율성**을 극대화 할 수 있다.

이를 위해서는 **로직이 Class A의 Data를 건드리는 것이 아니라(Data를 건드리는 순간 변경에서 자유롭지 못하다)**

**Class A의 '기능'을 추가/수정 하는 방향이 바람직하다**.

따라서 **캡슐화**는 유지보수 비용을 감소시켜주는 **객체지향의 전략 중 하나**이다.

<br/>

\---

위 내용은 '인프런' 최범균 강사님의 객체지향 입문 강의 내용을 요약 정리한 것임을 밝힙니다.

(https://www.inflearn.com/)



