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

