---
layout: post
title: "[Java] 가비지 컬렉션"
subtitle: "Garbage Collection의 전략"
background: '/img/bg_technology.jpg'
categories: technology-javaandpython
---


C와 비교했을 때 Java의 강점 중 하나가 바로 가비지 컬렉션이다.

맨 처음 Java를 배울 때 '가비지 컬렉션'이라는 단어를 처음 들었을 때 떠오른건 '휴지통'이었다.

쓰레기를 모은다니까...

지금은 어느정도 개념은 알긴 하지만, 

그래도 좀 더 깊은 내용을 확실히 정리해보고 싶어서 포스팅해보기로 했다.

---



#### 개념

JVM은 스택, 힙, 클래스 영역 등으로 메모리 영역이 분리되어있다.

그중 **Heap 영역에는 new 키워드로 생성된 객체 instance의 정보가 저장된다.**

그리고 어플리케이션이 실행되다 보면, 이 instance들이 경우에 따라 **Unreachable**하게 될 수 있다.

앞으로 접근 할 수도 없는 이 쓸데없는 instance들이 메모리 공간을 차지하고 있으면 

메모리의 낭비(**Memory leak)**가 일어나므로, 

결국 프로그램이 **OOM(Out Of Memory)**에 의해 종료되는 현상이 나타난다.

(메모리 부족으로 인해 프로그램을 종료시켜주는 것은 운영체제의 역할이다...)

더 이상 사용하지 않는 객체를 메모리에서 해제시켜주는 것이 Garbage Collection의 역할이다.

C나 C++과 같은 언어는 OS를 통해 메모리에 직접 접근하여 공간을 확보하기 때문에 

공간의 해제도 프로그래머의 몫이지만(free() 함수 등), 

Java는 **JVM이 메모리 공간을 점유하여 빌려쓰는 형태**이기 때문에,

메모리의 해제도 **JVM내부 로직이 알아서 작동하여 메모리를 해제하여주므로,**

**프로그래머는 메모리 누수를 신경쓸 필요가 없다**.

(반대로 말하면 JVM의 GC 최적화가 잘 이루어져야만 성능이 좋아진다는 뜻...)

---

#### 원리

Heap에 생성된 객체의 **주소값**을 가리키는 참조변수는 **Stack**에 위치한다.

만약 아래와 같은 코드가 있다면,

```java
public class HeapTest {
    public static void main(String[] args) {
        Member member1 = new Member("Test 1");
        Member member2 = new Member("Test 2");
        System.out.println("member1 : " + member1);
        System.out.println("member2 : " + member2);

        Member member3 = member1;
        System.out.println("member3 : " + member3);
    }
}

class Member {
    String name;

    public Member(String name) {
        this.name = name;
    }
}

```

member1, member2, member3는 각각 객체를 가리키는 **참조변수**이며,

콘솔 출력 결과는 아래와 같다.

```
member1 : ETC.Member@34a245ab
member2 : ETC.Member@7cc355be
member3 : ETC.Member@34a245ab
```

보다시피 member1, member2의 주소값은 다르나,

member1, member3의 주소값은 같다.

두 참조변수가 **동일한 객체**를 가리키고있기 때문에, **동일한 객체주소값**이 출력된 것이다.

(객체 주소값은 기본적으로 16진수로 출력된다. System.identitiHashCode() 함수를 쓰면 10진수로 출력 가능 )

기본적인 참조변수와 객체의 관계는 저렇고...

가비지 컬렉터가 작동하기 위해서는 당연히 **현재 Unreachable한 객체는 무엇인가**를 **스캔**할 필요가 있다.

스캔해보지 않으면 컴퓨터가 어떻게 판단하겠는가.

GC의 기본적인 스캔 및 지우기 메커니즘은 아래와 같다.

  - GC가 메모리 해제를 시킬 객체를 탐색하고 **Mark**한다
  - Mark된 대상 객체를 **Sweep**한다.

즉, 기본적으로 GC의 메커니즘은 **Mark and Sweep**이다.

(물론 GC를 위한 다른 알고리즘들도 많으나 가장 기본적인 전략만 살펴보자)

<br>

---

### Mark-Sweep의 Basic Strategy

**Tree와 BFS**를 떠올려보자.

Object를 Node, 참조되고 있는 상태를 Link로 본다면,

GC가 찾을 Unreachable한 Object는 

**Root Node에서 출발하여 수행된 BFS에 의해 Mark되지 않은 Object**라고 볼 수 있을 것이다.

그럼 Root는 어떻게 알 수 있는가?

자바에서 참조할 수 있는 경우는 4가지만 존재하는데, 그 중 Root가 될 수 있는 경우는

- Static 변수에 의해 참조되는 객체 (Static은 변하지 않으므로)
- Java stack영역 변수에 의한 참조 (참조의 시발점)
- JNI에 의해 생성된 객체에 의한 참조

의 세 경우가 있으며,

**다른 Object가 참조하는 Object**는 참조 Object가 해제되면 자기자신도 Unreachable되기 때문에

root set 에서 제외된다.

<br>

> Tip : JNI(Java Native Stack)이란 ? 
>
> Natice Method Stack이란 자바가 아닌 언어에서 제공되는 운영체제 메소드들로,
>
> C나 C++로 이루어진 메소드들을 Java에서 호출하도록 만들어진 규약을 JNI라 한다.
>
> Java는 WORA의 운영체제 독립성을 지니지만, 때로는 네이티브 코드들을 통해 운영제체를 잘 제어해야하는 경우가 있기 마련이다(하드웨어 제어, 속도 등).

<br>

Root set이 정해졌으면, 거기서부터 시작하여 객체들을 그래프 순회하듯 쭉 Scan한다.

Scan 대상이 되는 Object를 마킹한 후, 

Mark되지 않는 대상은 메모리에서 해제하면 끝이다.

---

### Improved Mark - Sweep Strategy

그런데...GC자체가 메모리가 부족해서 수행하는 녀석인데,

그 많은 객체를 전부 스캔하고 Mark하기에는, 

그것도 중간중간에 프로그램을 잠시 멈추었다가 매번 Full Scan을 돌린다면,

너무 비효율적이지 않을까?

따라서 이를 개선한 새로운 알고리즘들이 등장했다.

먼저, JVM은 Heap의 영역을 다음과 같이 나눈다.

![image](https://nirakarmohanty.files.wordpress.com/2017/05/gc-updated.png)

(출처 : https://nirakarmohanty.files.wordpress.com/)

(참고로 위 그림은 Java 7 기준인 것 같다. Java 8  부터는 Permanent 영역이 사라졌다고 함)

그리고, Garbage Collection을 **Minor GC**와 **Major GC**로 나눈다.

- Minor GC : Young Generation (생성된 지 얼마 되지 않은 객체들이 있는 공간)을 Scan하여 Unreachable한 객체들을 제거
  - 최초 객체들은 Eden(에덴동산?) 영역으로 이동
  - Eden에서 살아남은 객체들은 Survivor 1, 2를 번갈아가며 이동
  - 거기서조차 살아남은 애들은 Tenured(종신..?)영역으로 간다.

- Major GC : Old Generation(생성된 지 오래된 객체들이 있는 공간) 및 Young Generation을 **Full Scan**하여 제거
  - Old영역조차 메모리가 가득 찼을 때 실행.
  - Full Scan할 때 GC스레드를 제외한 나머지 스레드는 모두 작동을 멈춘다.
  - 이 상태를 **Stop-the-World**라 한다.

이렇게 객체 등급이 승격(...)하는데,

어떻게 **오래 살아남았다**를 판단할까?

각 객체는 **Counter bit**를 가지는데, Scan이 일어나고 살아남게 될 때마다 bit값이 1 증가한다.

그리고 JVM에 설정된 임계값을 넘으면, 해당 객체는 **승격하여 영역을 이동한다**.

(이  JVM설정은 변경이 가능하다고 한다)

이렇게 Minor GC와 Major GC를 나누어서 실행하면 매번 Full Scan을 하는 것 보다 효율성이 훨씬 올라간다.

대청소 한 번 하는 것 보다 작은 청소를 여러번 해주는 것이다.

위 방식을 실행하는 두 가지 전제가 있는데,

- 대부분의 객체는 생성되고나서 얼마 되지 않아 unreachable하게 된다.
- Old 객체는 Young 객체를 참조할 일이 거의 없다.

따라서 Young 영역 객체를 자주 스캔하여 청소하는 것이 유리하다.

<br>

















