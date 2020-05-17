---
layout: post
title: "JAVA 파헤치기 (Part 3)"
subtitle: "JVM의 메모리 구조"
background: '/img/bg_technology.jpg'
categories: technology

---


### Java의 Memory Structure



Java는 JVM을 이용하여 운영체제 독립적으로 작동하는(WORA) 언어다.

JVM은 가상머신, 즉 운영체제 위에서 돌아가는 또 다른 형태의 '가상화'된 공간이라고 생각하면 된다.

이 공간 역시 일정 메모리를 점유하고 있으며, 메모리 공간을 분류하여 클래스 구성요소별로 할당시킨다.

따라서 메모리 관점에서 효율적인 Java 프로그래밍을 하기 위해서는 이 내용을 머릿속으로 그릴 수 있어야 한다.

나 또한 Java를 주력 언어로 사용하고 있는 유저로, 그동안 어렴풋이 알고있었던 내용들을 명확히 정리하고자 본 포스팅을 올린다.



---

### Runtime Data Area



고수준 언어인 Java로 객체 및 변수를 선언, 메소드를 정의하고 javac 명령어를 통해 컴파일하면 .class 파일이 생성된다. 

Java Byte Code인 .class 파일은 JVM에 들어가 비로소 'read'되는데, 

이 클래스 파일은 **Class Loader System**에 의해 **Runtime Data Area**에 Load된다.



JVM은 C와 같은 네이티브 코드와는 달리, 

운영체제로부터 필요한 메모리를 할당받아 일정 영역으로 나누어 제어하게 된다.

즉 메모리를 제어하는 운영체제로부터, 본인이 제어할 메모리 영역을 또 다시 분배받는 것이다.

이 Data Area영역은 또다시 5개로 나누어지는데, 

이는 각각 **Method Area, Heap, Stack, PC Register,  JVM Stacks** 이다.



#### #Method Area : Class information

**클래스**에 대한 정보가 기록되는 영역이다.

클래스 멤버변수명, Data type, Field Information 등 필드정보 및 메소드명, 리턴타입, 파라미터 등 메소드정보가 함께 저장된다.

**Static 변수** 또한 이 영역에 저장된다.



#### #Stack Area : Class information

작성중



#### #Heap Area : Class information

**객체**에 대한 정보가 저장되는 곳이다. new 키워드로 생성된 object의 정보가 이곳에 저장된다고 보면 된다.

모든 객체타입은 전부 Heap에 저장된다고 보면 된다(Array, List, HashMap, Queue 등등..)

자바의 **Garbage Collector**는 이 Heap 영역에서 '**unreachable**'한 객체들을 알아서 제거해준다.

가령 아래 코드에서,

```java
public class UnreachableObject {
    public static void main(String[] args) {
        Test test = new Test("instance");
        test = null;
    }
}

class Test {
    String x;

    public Test(String x) {
        this.x = x;
    }
}
```



#### #Method Area : Class information

작성중



---





JVM은 이 때 운영체제로부터 필요한 메모리를 할당받아 일정 영역으로 나누어 제어하게 된다.

C 등의 Native code들은 실행시 운영체제가 해당 프로그램을 위한 메모리를 즉각 할당하며, 메모리 공간은 Static(전역번수) 영역, Heap 영역, Stack 영역에 따라 동적으로 변한다.



반면 Java는 운

------

