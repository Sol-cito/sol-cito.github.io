---
layout: post
title: "[Java] 자바의 예외(Exception) 처리"
subtitle: "Throwable, Runtime Exception, Checked Exception"
background: '/img/bg_technology.jpg'
categories: technology-javaandpython
---

예외처리는 프로그램의 구성에 있어 굉장히 중요하다.

프로그래밍을 시작한지 거의 2년이 다 되어가는데, 

예외상황을 잘 고려하여 만드는 것은 항상 어렵다.

알고리즘 문제를 풀 때도 예외 케이스에 걸리는 경우가 매우 빈번하며,

단순 프로그램을 짤 때에도 생각지도 못한 예외 상황이 많이 발생하여 사후 처리에 애를 먹은 경험이 있다.

사실 이건 누구나 하는 경험이라고 생각하는데, 

예외 상황의 경우의 수를 하나하나 전부 생각하는것은 생각보다 귀찮은 일이고,

에너지가 많이 소모되는 일이기 때문이다.

하지만 그만큼 **프로그램의 완성도**를 위해서는 필수적인 과제이기도 하다.

따라서 오늘은 Java의 예외처리 프로그래밍에 대해 알아보도록 하겠다.

---

#### Exception, Error and Throwable

던지는 객체 **Throwable**을 상속한 객체에는 **Error(오류)**와 **Exception(예외)**두 가지가 있으며,

두 가지 객체 모두 **throw** 키워드로 던지며 **try & catch**로 처리할 수 있다.

Throwable의 상위 객체는 모든 객체의 조상인 Object이다.

> Error는 응용프로그램 수준에서 발생하기 보다는 시스템의 비정상적인 상황에 의해 발생하는 문제이므로,
>
> 응용프로그램 개발자는 어플리케이션 개발이서 오류에 대한 처리는 거의 신경쓰지 않는다.
>
> 물론 코딩 자유도는 열려있다.

아래 그림이 이해를 도울 것이다.



![image](https://www.javamex.com/tutorials/exceptions/ExceptionHierarchy.png)

(출처 : https://www.javamex.com/tutorials/exceptions/exceptions_hierarchy.shtml )

<br/>

보다시피 Exception, 즉 응용 개발자가 처리해야하는 예외에는 두 가지 종류가 있다.

**Checked Exception**과 **Runtime Exception**의 두 가지 예외가 그것이다.

<br/>

#### **Checked Exception**

Checked Exception의 종류에는 위 그림에서 보다시피 **IOException, SQLException...**등이 있으며, 이는 말 그대로

"*프로그램이 미리 check하는 로직으로, 예외처리가 강제되는 것* "이다. 

아래 코드를 보면, 

```java
import java.io.FileWriter;

public class Practice {
    public static void main(String[] args) {
        FileWriter fw = new FileWriter("test.txt"); // 빨간줄이 표시됨
    }
}
```

FileWriter의 생성자에 밑줄이 그이면서 컴파일이 되지 않는다.

실행 시 아래 메시지가 콘솔창에 출력된다.

> java: unreported exception java.io.IOException; must be caught or declared to be thrown

말 그대로 **IOException이 예상되므로, catch하던가 throw하던가 암튼 처리해** 라는 뜻이다.

FileWriter는 외부 파일을 읽어오는 클래스이기 때문에, 파일을 읽는 시스템콜을 요청할 것이다.

그런데 만약 외부에 파일이 존재하지 않는다면 오류가 발생할 것이므로, 미리 그 상황에 대비하라는 메시지를 출력하는 것이다.

SQLException도 마찬가지로, Java는 해당 쿼리문이 올바르게 실행될 것인지를 알지 못하기 때문에

DAO같은 데서 쿼리를 수행할 때 미리 대비하라고 예외처리를 강제하는 것이다.

<br/>

#### Runtime Exception

프로그램은 예외를 항상 예측할 수는 없다.

큰 프로그램에서의 연산은 정적인 경우보다 동적인 경우가 훨씬 많기 때문에,

항상 parameter나 연산 조건에 변화가 생기기 마련이다. 

따라서 이 경우는 프로그래머가 예외 상황을 잘 이해하고 그에 따른 방어 프로그래밍을 해주어야 하며,

**이것이 바로 프로그래머의 실력이라 할 수 있다.**

Runtime Exception 에는 대표적으로 내가 알고리즘 문제를 풀다가 자주 마주치는(...) **IndexOutOfBoundsException**,

Null값이 리턴되거나 파라미터로 주어졌을 때 연산에 실패하는 **NullPointerException** 등이 대표적이다.

프로그래머는 해당 예외가 발생할 것으로 예상되는 부분의 처리를 해주어야 하는데,

그 방법은 두 가지가 있다.

##### 1. try ~ catch 로 스스로 예외를 처리한다.

```java
public class Practice {
    public static void main(String[] args) {
        int a = 10;
        int b = 0;
        devide(a, b);
    }

    public static int devide(int a, int b) {
        int answer = 0;
        try {
            answer = a / b; // ArithmeticException 발생
        } catch (Exception e) {
            e.printStackTrace(); // 여기서 catch 된다.
        }
        return answer;
    }
}
```

int b가 0이므로 devide 메소드에서 Runtime Exception이 발생한다.

devide 메소드는 착하니까 일을 미루지 않고 자기 선에서 문제를 해결한다.

예외의 종류가 다양하므로, catch 블록 또한 **다중 catch블록**으로 만드는 것이 가능하다.

```java
public class Practice {
    public static void main(String[] args) {
        int a = 10;
        int b = 0;
        devide(a, b);
    }

    public static int devide(int a, int b) {
        int answer = 0;
        try {
            answer = a / b; // ArithmeticException 발생
        } catch (ArithmeticException e) {
            e.printStackTrace(); // 여기서 catch 된다.
        } catch (NullPointerException e) {
            e.printStackTrace();
        } catch (Exception e) { // 위의 두 예외에서 걸러지지 않은 예외가 catch된다.
            e.printStackTrace();
        }
        return answer;
    }
}
```

이렇게 차례대로 **체에 거르듯이** 다중 블록을 이용하여 구성할 수도 있다.

주의할 점은, **상위 블록의 범위가 하위 블록보다 크다면 하위 블록이 필요없어지기 때문에 컴파일 에러가 발생한다.**

<br/>

이렇게 혼자 착하게 자신의 똥을 치우는 것이 좋다고 생각될 수도 있지만....

아래의 경우를 보자.

```java
public class Practice {
    public static void main(String[] args) {
        selectFood();
        giveMoney();
        receiveChange();
    }

    public static void selectFood() { // 음식을 고른다
        try {} catch (Exception e) {}
    }

    public static void giveMoney() { // 돈을 준다
        try {} catch (Exception e) {}
    }

    public static void receiveChange() { // 거스름돈을 받는다
        try {} catch (Exception e) {}
    }
}
```

음식을 고르고 - 돈을 지불하고 - 거스름돈을 받는 각각의 메소드들이 책임감있게 예외를 처리하고 있다.

그러나 위와 같은 경우, receiveChange() 에서 예외가 발생하면 문제가 발생한다.

**이미 main메소드에서 selectFood(), giveMoney() 는 처리가 완료되었음에도 불구하고,**

**receiveChange()의 예외가 바로 catch가 되면 그 곳에서 프로그램이 종료되기 때문이다.**

즉 **트랜잭션**의 관점에서 위와 같은 예외처리는 큰 문제가 발생할 수 있다.

위와 같이 메소드가 체인처럼 연결되어있는 경우엔 트랜잭션 관리를 위해 아래와 같은 예외처리가 요구된다.

<br/>

##### 2. 아몰랑 하고 자신을 호출한 부모에게 throw 해버리기

extends, implements 처럼 **throws** 키워드로 자신에게서 발생한 메소드를 던질 수 있다.

어디로? 자신을 호출한 부모에게로...

다시 한번 아래 코드를 보자.

```java
public class Practice {
    public static void main(String[] args) {
        try {
            selectFood();
            giveMoney();
            receiveChange();
        } catch (Exception e) { // 위 3개 메소드의 예외를 모두 여기서 처리함
            e.printStackTrace();
        }
    }

    public static void selectFood() throws Exception {
    }

    public static void giveMoney() throws Exception {
    }

    public static void receiveChange() throws Exception {
        int x = 10;
        int y = 0;
        System.out.println(x / y); // 예외 발생 구간
    }
}
```

3개의 메소드가 모두 throws Exception을 하고 있으며, 

receiveChange()에서 ArithmeticException이 발생한다.

그럼 receiveChange()는 자신을 호출한 부모인 giveMoney()로 예외를 던지고,

giveMoney()는 selectFood()에게 다시 던지고,

결국 예외는 최종적으로 main 메소드로 도착하여 그 곳의 catch문에서 처리된다.

**이 방식이라면 셋 중에 어느 하나에서 예외가 발생하더라도 안전한 트랜잭션을 보장할 수 있다.**

어느 하나라도 잘못되면 프로그램이 최초 호출 위치로 되돌아오기 때문이다(**Roll-back**).

<br/>

예외처리는 안전한 프로그램의 완성을 위해 너무나도 중요하다.

사실 나도 여태까지 프로그래밍을 배우면서 **완성**에 초점을 맞추고 기뻐했었지,

예외상황을 가정하고 처리를 생각하는건 귀찮기도 하고 별로 신경을 안쓴 경우가 대부분이었다.

지금와서 돌아보면, 처음부터 예외상황을 생각하지 않고 프로그래밍 하는 것이

**오히려 나중에 시간과 노력을 배로 들이게 하는 원흉이었다..**





