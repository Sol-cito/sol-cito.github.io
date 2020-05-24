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

### 운영체제로부터 메모리 공간을 할당받는 JVM



C 등의 Native code들은 실행시 운영체제가 해당 프로그램을 위한 메모리를 즉각 할당하며, 메모리 공간은 Static(전역번수) 영역, Heap 영역, Stack 영역에 따라 동적으로 변한다.

반면 JVM은 운영체제로부터 필요한 메모리를 할당받아 그 공간을 다시 일정 영역으로 나누어 제어하게 된다.

즉 메모리를 제어하는 운영체제로부터, 본인이 제어할 메모리 영역을 또 다시 분배받는 것이다.

메모리를 할당받아 구성하는 공간이 바로 JVM의 **Runtime Data Area**다.

(그렇다면 OS가 JVM에 '**얼마만큼**'의 메모리 공간을 부여할 지는 어떻게 계산할까? 추후에 한번 알아보면 좋을 것 같다)



---

### Runtime Data Area

고수준 언어인 Java로 객체 및 변수를 선언, 메소드를 정의하고 javac 명령어를 통해 컴파일하면 .class 파일이 생성된다. 

Java Byte Code인 .class 파일은 JVM에 들어가 비로소 '**read**'되는데, 

이 클래스 파일은 **Class Loader System**(메모리에 .class파일 적재)에 의해 **Runtime Data Area**에 Load된다.



이 Data Area영역은 또다시 5개로 나누어지는데, 

이는 각각 **Method Area, Stack, Heap, PC Register,  JVM Native Stacks** 다.



#### # Method Area : Class information

**클래스**에 대한 정보가 기록되는 영역이다.

클래스 멤버변수명, Data type, Field Information 등 필드정보 및 메소드명, 리턴타입, 파라미터 등 메소드정보가 함께 저장된다.

**Static 변수, final 변수**  또한 이 영역에 저장된다.



#### # Stack Area : Class information

스레드 하나 당 Stack Area가 하나씩 존재한다.

 -> 생각해보면 당연하다. 멀티 스레드 환경에서 각 스레드별로 같은 Stack 을 공유한다면 난리가 날 것이다...

The Element of computing system 의 7,8장 내용을 복기해보자.

자바 프로그램 내에서 Method가 하나 호출될 때마다 Stack 영역에는 그 이전 Method의 지역변수, 인수, 메모리 내 return 주소값 등이 frame 형태로 저장된다.

frame이 저장된 후 stack 에 다시 호출한 method의 지역변수, 인수 등이 기록된다. 즉 새로운 frame이 만들어진다.

이렇게 만들어진 새로운 frame은 해당 method가 종료되면 stack pointer의 이동으로 소멸된다. 마치 아래 그림처럼 말이다.

<img src="https://upload.wikimedia.org/wikipedia/commons/8/8a/ProgramCallStack2_en.png" alt="File:ProgramCallStack2 en.png - Wikimedia Commons" style="zoom:25%;" />

(Image source : https://commons.wikimedia.org/wiki/File:ProgramCallStack2_en.png)

자바에서 **변수나 객체를 초기화 하지 않는다면**, stack pointer가 특정 주소를 가리키기만 하고 값은 텅 빈채로 남아있을 것이다.



#### # Heap Area : Class information

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

Test 객체를 만들고 "instance"라는 String 인스턴스를 생성한 후 참조시켰으나,

그 다음줄에 바로 객체를 null로 만들어 "instance"인스턴스를 아무도 가리키지 않게 만들었다.

이 경우, "instance"는 아무도 가리키지 않는 인스턴스, 쓰레기 데이터가 되어 가비지 컬렉션의 타겟이 된다.

(가비지 컬렉터의 기능 및 원리에 대해서는 추후 포스팅 예정)



#### # PC Register

Stack area와 마찬가지로 쓰레드마다 하나씩 존재하며, 해당 Stack area에서 다음번에 수행될 명령의 주소값을 저장한다.

CPU의 프로그램 카운터와는 다른, JVM내의 PC라 보면 된다.



#### # JVM Native Stacks

C, C++와 같은 네이티브 코드로 이루어진 영역으로, 하드웨어를 제어하기 위한 메소드가 들어있다.

Java Stack과 Native Stack은 분리되어 작동하며, Java Stack에서는 하드웨어 제어가 불가능하기 때문에 필요시 Native Stack으로 가서 하드웨어 제어를 수행하였다가 다시 Java Stack를 호출하여 복귀하는 방법을 사용한다.



---

