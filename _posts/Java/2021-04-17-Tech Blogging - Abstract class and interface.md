---
layout: post
title: "[Java] 추상클래스와 인터페이스"
subtitle: "관계, 기능, 상속, 다형성"
background: '/img/bg_technology.jpg'
categories: technology-javaandpython
---

Java 에서 두 개념은 **사용자에게 강제하는 것은 같으나, 존재 이유가 다르다.**

추상클래스와 인터페이스의 공통점은 아래와 같다.

> 사용자에게 미완성 메소드의 구현을 강제하게 한다.

먼저 **추상클래스**의 생김새를 보면,

```java
abstract class Practice { //메소드에 abstract 지시자가 있으므로 class에도 필요함
    public abstract void sum(int x, int y); // abstract 지시자 필요.
    // 추상메소드는 {} body가 없다

    public int minus(int x, int y) { //일반 메소드
        return x - y;
    }
}

// Practice 상속
class Temp extends Practice{
    @Override
    public void sum(int x, int y) {
      // 추상 메소드 구현
    }
}
```

보다시피 추상클래스는 **추상 메소드**가 **1개 이상** 존재하는 클래스이다.

위 예시에서는 minus는 추상메소드가 아님에도 해당 클래스는 추상메소드로 기능하는 것을 볼 수 있다.

Practice 추상클래스를 상속한 자식 클래스는 추상 메소드 sum을 반드시 구현해야하며(그렇지 않으면 컴파일 에러 발생)

구현은 **메소드 오버라이딩**으로 이루어진다 (@Override 어노테이션).

<br/>

다음으로 **인터페이스**를 살펴보면

```java
interface PracticeInterface {
    void printString(); // 추상클래스와 마찬가지로 {} body가 없다
    
    void printInt();
}

/* Practice를 상속 */
class Temp extends Practice implements PracticeInterface {
    @Override
    public void sum(int x, int y) {
    }

    @Override
    public void printString() { // 추상 클래스 메소드
    }
   
    @Override
    public void printInt() { // 인터페이스 메소드
    }
}

/* Temp를 상속 */
class Temp2 extends Temp implements PracticeInterface{
    public Temp2(){}
}
```

인터페이스 또한 해당 인터페이스의 추상 메소드 구현(implement)을 강제한다.

인터페이스는 1개 이상 메소드의 구현을 강제 가능하며(위 예시처럼 메소드가 2개 이상일 때는 2개 전부 구현해야 함),

따라서 **implements**지시자로 해당 인터페이스의 메소드를 구체적인 클래스에서 구현한다.

위 예시에서 Temp 클래스는 Practice를 상속하고, PracticeInterface 또한 구현하기 때문에,

각각의 추상메소드를 모두 Override하여 구현이 강제되었다.

참고로, 맨 아래 **Temp2** 클래스는 **Temp** 클래스를 상속하였고, PracticeInterface를 implements 하는데,

**Temp 클래스에 이미 해당 인터페이스 메소드가 구현되어 있으므로,**
**Temp2 클래스는 오버라이딩이 강제되지 않는다.**

상속의 개념을 알고 있다면 당연한 소리다.

**자바8부터는 default 메소드 및 static 메소드 선언도 가능하게 되었다.**

([Java 8 - 모던 자바의 특징](https://sol-cito.github.io/technology-javaandpython/2021/04/08/Tech-Blogging-Java-8) 포스트에서 다룬 적 있다)

인터페이스에서 default나 private 접근제어자는 특수한 목적에 따라 사용되기 때문에, 

기본적인 인터페이스 추상 메소드의 접근 제어자는 **생략해도 default가 아닌 public으로 기능한다.**

인터페이스끼리도 상속이 가능하다.

가령,

```java
interface PracticeInterface {
    void printString();
}

interface PracInter2 extends PracticeInterface{ // 인터페이스 상속
    void doublePrint();
}
...
// PracticeInterface 의 추상메소드만 구현하면 됨
class Temp extends Practice implements PracticeInterface {
    @Override
    public void sum(int x, int y) {}

    @Override
    public void printString() {}
}

// Practice 클래스 상속으로 printString()은 따로 구현할 필요 없으나,
// PracInter2 인터페이스를 구현하므로 추가적으로 doublePrint()메소드를 구현해야 함
class Temp2 extends Temp implements PracInter2{
    public Temp2(){}

    @Override
    public void doublePrint() {}
}
```

PracInter2는 인터페이스를 상속하였으므로, 

해당 인터페이스를 implements 하는 클래스는 PracInter2의 상위 interface의 메소드도 함께 구현해야 한다.

위 예시의 Temp2 클래스는 이미 Temp를 상속하여 PracInter2의 상위 메소드를 구현할 필요가 없기 때문에,

PracItner2의 메소드만 구현하였다.

이처럼 설계에 따라 다양한 방식으로 Interface를 활용할 수 있다.

<br/>

#### 존재 목적의 차이 - 상속 vs 다형성

얼핏 봐서는 두 개념에 차이가 없어보인다.

그러나 **객체지향**의 관점에서 두 개념은 큰 차이를 지니는데,

내 생각에 두 개념의 차이는 곧 **extends와 implements의 기능적 차이**라 생각한다.

<br/>

#### 추상클래스 - 공통적인 것을 추출하여 추상적으로 개념화 (상속)

상속, 즉 extends는 Java에서 implements 보다 **더욱 엄격한 개념**이다.

상속은 말 그대로 부모의 전체적인 틀을 물려받아 그 기능 및 구성을 **확장**하는 것이다.

**인간**과 **갑동, 을동, 철수** 클래스를 예로 들어보자.

'인간' 클래스는 갑동, 을동, 철수 등 개인 클래스의 **공통 조상**이다.

그러나 '인간'클래스는 사실 **현실에 존재하지 않는, 추상적인 개념**이다(플라톤의 이데아와 같은..).

이러한 경우, 우리는 갑동, 을동, 철수 등 개개인의 클래스를 만들 때 

'먹다', '자다', '입다' 등 인간의 공통적인 행동을 일일이 따로따로 구현할 필요 없이,

**'인간' 추상클래스를 만들고 각각의 행동(메소드)을 추상메소드로 규정**하여,

**각각의 개인 클래스가 해당 추상클래스를 '상속'**하도록 설계하고, 

개개인의 특성에 맞는 행동양식(메소드)를 **각각 알아서 구현하도록** 유도하는 것이다.

따라서 추상클래스는,

**자식 클래스로 메서드 구현의 책임을 '위임'** 하고,

**자식 클래스들이 공통의 추상화된 기능을 가지도록** 설계하는 데 그 목적이 있다.

즉 추상클래스에서는 **자식클래스 A is 부모클래스 B** 의 관계가 성립한다.

<br/>

#### 인터페이스 - 상속관계가 없는 클래스라도 동일한 기능을 하도록 + 다형성

인터페이스는 **동일 기능의 제공**이 그 핵심이다.

위에서는 인간 << 갑동, 을동, 철수 의 예시로 설명하였지만,

인터페이스는 '상속' 관계가 없는 인스턴스들 끼리도 **서로 동일한 기능**을 구현해야 할 때 사용한다(내용은 다르다).

즉 추상클래스에 비해 객체들의 관계가 **훨씬 느슨하다.**

Java의 **List Interface**를 생각해보자.

``` java
public class Main {
    public static void main(String[] args) {
        List<String> list1 = new ArrayList<>();
        List<String> list2 = new LinkedList<>();
    }
}
```

위 코드의 ArrayList와 LinkedList 는 각각 하나의 **클래스 인스턴스**이며,

둘 다 **List 인터페이스**를 구현한 **구현체**이다.

즉, ArrayList와 LinkedList의 **데이터 타입**은 **클래스가 아니라 인터페이스**이다.

**(두 클래스가 인터페이스를 구체적으로 구현하고 있기 때문)**

두 클래스 모두 최상위의 **AbstractCollection**클래스를 '상속'하고 있다는 점은 같으나(즉 조상은 같으나),

두 클래스가 가진 contains(), add() 등 몇몇 **공통 메소드**는 조상 클래스가 아닌 **Interface**가 제공하는 기능으로부터 나온다.

왜냐하면 사실 조상 추상클래스 **AbstractCollection**또한 **Collection 인터페이스**를 구현하기 때문이다.

```java
public abstract class AbstractCollection<E> implements Collection<E> {
	...
}
```

(참고로 List 는 Collection 인터페이스를 상속하고, Collection는 최종적으로 Iterable 인터페이스를 상속함)

이처럼 특정 인스턴스들이 **동일한 기능**을 가지고 있으며 상속 관계는 아닐 때, 인터페이스로 유연한 설계가 가능하다.

인터페이스는 **구체적인 메소드 명세를 제공하고 있지 않으므로, 동일 메소드가 다양한 기능을 가질 수 있기 때문이다**.

이러한 인터페이스의 특징은 **다형성**과 밀접한 관계가 있다.

> 다형성은 하나의 클래스가 다양한 동작 방식을 지니고 있는 것.

다음 예시를 보자.

```java
/* 공격, 방어 인터페이스 */
interface Attack {
    void attack();
}

interface Defense {
    void defense();
}

/* 두 인터페이스를 구현한 구체적인 클래스 */
class Player implements Attack, Defense{
    @Override
    public void attack() {}
    	
    @Override
    public void defense() {}
}

/* Main메소드에서 인스턴스 생성 */
public class Main {
    public static void main(String[] args) {
        Player p1 = new Player(); // 둘 다 가능
        p1.attack();
        p1.defense();

        Attack p2 = new Player();
        p2.attack();
        p2.defense(); // 에러

        Defense p3 = new Player();
        p3.attack(); // 에러
        p3.defense();
    }
}
```

**Player** 클래스는 Attack, Defense 인터페이스를 둘 다 구현하고 있다.

따라서 해당 클래스 인스턴스인 p1은 attack(), defense() 메소드를 둘 다 가질 수 있다.

그러나 **Attack, Defense 인터페이스를 '데이터 타입'으로 구현**한 구현체인 p2, p3의 경우,

두 인스턴스가 사용할 수 있는 메소드는 **데이터 타입 인터페이스의 메소드로 제한된다.**

결과적으로 동일한 클래스 인스턴스인 p1, p2, p3 의 각각의 기능이 **인터페이스로 인해 다양한 모습을 가지게 된다는 것이다.**

이것이 바로 **인터페이스가 제공하는 또다른 '다형성'이며, 이로 인해 유연한 설계 및 변경이 가능하다.**

<br/>

상속, 추상화, 인터페이스, 추상클래스에는 공통되는 개념도, 다른 개념도 있다.

이를 목적에 맞게 적절히 취사선택하는 것은 오롯히 설계자와 개발자의 역할이므로,

유연하고 확장성 높은 설계를 위해서는 이러한 개념에 대해 잘 숙지하고 있어야만 한다.