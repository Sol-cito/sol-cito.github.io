---
layout: post
title: "[OOP] OOP 바로알기 (2)"
subtitle: "추상화의 유연함"
background: '/img/bg_technology.jpg'
categories: technology-oop
---

### 추상화

객체지향 프로그래밍을 하는 데 있어, **캡슐화**와 더불어 가장 중요한 전략이 바로 **추상화**이다.

이 추상화는 **다형성** 개념과 밀접한 연관성을 지닌다.

다형성이란 **한 객체가 여러가지 '타입'을 갖는 것**을 의미하는데,

한 객체가 여러가지 타입을 갖기 위해서는 **타입 상속**이 되어야 한다.

즉, 상위 객체를 상속한 하위 객체는 상위 타입과 하위 타입 **둘 다 가질 수 있는 다형 객체**가 되므로, 다형성을 지닌다.

<br/>

그러면 이러한 상속의 대상인 **상위 타입 객체**는 어떻게 설정하는가?

상속의 대상인 인터페이스나 추상클래스는 특정 기능의 **공통적인 성질**을 뽑아냄으로서 설정되며,

이러한 과정이 바로 **추상화**라 할 수 있다.

<br/>

##### 추상화 없이 Concreate Class를 사용했을 때의 issue

만약 어떤 주문 상황이 발생했을 때, 메일로 그 상황을 통지해야 하는 기능을 개발했다고 생각해보자.

```java
class EmailNotifier{
    // Email로 Notify를 하는 클래스
}

...
    
class OrderService{
 	EmailNotifier emailNoti = new EmailNotifier();
    
    public void order(){
        // 주문 관련 로직
		emailNoti.notify();
    }       
}
```

위와 같이 **Concreate Class(인터페이스가 아닌 클래스)**를 사용해서 Email Notification을 줄 수 있을 것이다.

그런데...

**요구사항이 변경**되어서 Kakao Talk, SMS로도 Notification을 주어야 하는 상황이라면?

```java
class OrderService{
 	EmailNotifier emailNoti = new EmailNotifier();
    KakaoNotifier kakaoNoti = new KakaoNotifier();
    SMSNotifier smsNoti = new SMSNotifier();
    
    public void order(){
        // 주문 관련 로직
        if(email){
            emailNoti.notify();
        }else if(kakao){
            KakaoNotifier.notify();
        }else{
            SMSNotifier.notify();
        }
    }       
}
```

위와 같이 코드는 점점 길어지고, 

**주문 관련 로직의 내용이, 주문과 상관없는 Notification 관련 코드들로 가득 차는 결과가 발생한다.**

즉, 주객이 전도되고 해당 메소드의 기능이 모호해지는 것이다.

<br/>

사실 저 3개의 Concreate Class의 기능은 같다.  바로 **Nofify**의 기능이다.

만약 Nofity라는 공통 기능을 뽑아내서 하나의 클래스로 만들 수 있다면,

우리는 그 기능을 따로 **'관리'**할 수 있게 될 것이며, 변경에 있어서도 굉장한 유연함을 얻을 수 있다.

```java
public interface NotifierFactory{ // 추상 팩토리로 notifier '생성'기능을 추상화
    Notifier getNotifier(param);
    
    static NotifierFactory getInstance(){
        return new DefaultNotifierFactory();
    }
}

public class DefaultNotifierFactory implements NotifierFactory{
    public Notifier getNotifier(param){ // 인터페이스 메소드 구현
        if(param == "Email"){
            return new EmailNotifier();
        }else if(param == "Kakao"){
            return new KakaoNotifier();
        }else{
            return new SMSNotifier();
        }
    }
}

class OrderService {  
    public void order(){
        // 주문 관련 로직
        Notifier notifier = NotifierFactory.getInstance().getNotifier(param); // 팩토리에서 notifier 생성
        notifier.notify(); // notify 수행
    }       
}
```

위처럼 추상화를 이용해 구현한다면, 

만약 통지 방식을 바꾸라는 요구사항이 들어왔을 때 개발자는 

**DefaultNotifierFactory**를 수정하거나 **NotifierFactory**를 수정해야 하지만,

**개발자는 '주문' 관련 코드는 수정할 필요가 없어지는 장점을 얻을 수 있다.**

어차피 Notify라는 기능이 추상화되어 Order에서 수행되기 때문이다.

<br/>

자, 그럼 추상화는 언제 해야만 할까?

**추상화를 한다는 것은 '새로운 타입'이 생겨난다는 것이고, 이는 프로그램의 복잡도가 증가한다는 의미이다.**

따라서 잘못된 추상화는 복잡도만 증가시키는 결과를 가져오므로,

실제로 요구사항이 변경되어 **설계의 변경, 확장이 필요할 때** 개발자는 **추상화를 시도해야만 한다.**

그렇지 않다면 배보다 배꼽이 더 큰 상황에 마주할 것이다.

즉, **항상 '왜 추상화를 해야하는가'를 생각하면서 설계를 해야한다.**

<br/>

\---

위 내용은 '인프런' 최범균 강사님의 객체지향 입문 강의 내용을 요약 정리한 것임을 밝힙니다.

(https://www.inflearn.com/)



