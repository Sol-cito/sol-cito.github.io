---
layout: post
title: "[Java] String 파헤치기"
subtitle: "String, String Pool, StringBuilder, StringBuffer"
background: '/img/bg_technology.jpg'
categories: technology-javaandpython
---


자바의 String이라는 녀석에 대해 파헤쳐보자. 왜? 재미있으니깐...

> String은 '객체'다.

라는 말은 코딩을 배운 첫 날에 들어봤던 것 같다.

물론 그 당시에는 이해가 잘 안됐다. int는 네이티브 변수인데 String은 왜 아니지?

그 이유는 **컴퓨터는 0과 1의 bit를 가지고 데이터를 저장하기 때문**이라 할 수 있다.

문자'열'이란, 개별 글자의 조합으로 만들어지는 데이터다.

따라서 문자열의 수는 **무한하다**.

int는 4바이트, 즉 32비트이므로, Java에서는 2^32 만큼의 범위 표현이 가능하다.

그런데 그 경우의 수가 무한한 String은 그렇게 표현하는 것이 불가능하고, 

대신 **하나의 글자는 표현이 가능하다**.

하나의 글자를 숫자로 매핑하여(ex) 유니코드),

**<u>문자열</u>**을 **<u>숫자로 변환된 글자의 집합</u>**으로 표현한 것이 String이다.

따라서,

**String은 char 배열로 이루어진 하나의 '객체'**인 것이다.

---

#### String의 생성 방법 2가지

아래 코드를 보자.

```java
public static void main(String[] args) {
    String a = new String("Test"); // new
    String b = "Test"; // literal
    String c = "Test";
    System.out.println("테스트1 : " + (a == b));
    System.out.println("테스트2 : " + (b == c));
}
```

```
테스트1 : false
테스트2 : true
```

왜 두 테스트 결과가 다를까?

String을 생성하는 방법은 두 가지가 있다.

**문자 그대로(Literal) 생성하는 방법**과 **new 명령어로 객체를 생성하는 방법**의 두 가지인데,

두 방식에는 다음과 같은 차이가 있다.

- Literal 방식은 **Heap의 String constant pool (스트링 상수풀)**에 저장되며, 참조변수가 스트링 상수풀 주소를 가리킨다.
- new 생성자 방식은 **Heap에 일반 Object처럼 저장된다.**

String Constant Pool이란 '상수'라는 의미 그대로 '변하지 않는'다는 의미로, 

만약 리터럴 방식으로 선언된 String이 모두 "Cat"이라는 문자열을 지닌다면

해당 String의 참조변수들은 **모두 같은 String Pool 안의 "Cat" 주소를 가리킨다.**

테스트2가 true값이 나온 이유도, String b와 c가 둘 다 "Test"라는 값을 지니는데,

String pool 안에서의 "Test" 주소값이 같기 때문이다.

반면, 테스트 1이 false값이 나온 이유는 생성자를 가리키는 String a 는

String constant pool 외에 다른 Heap 영역의 메모리를 차지하고 있기 때문이다.

<br>

종합해보면, **Java에서 String은 '불변(immutable)'하는 특징을 지닌다**.

같은 문자열을 가진 String을 아무리 많이 리터럴로 생성하더라도,

그 개수만큼의 객체가 생성되는 것이 아니라, 똑같은 String pool의 주소를 가리키기 때문이다.

String의 불변성에 따른 이득은 무엇일까?

- **필요없는 overhead가 줄어들고, 프로그램의 성능을 높여준다.**
- String 값이 변하지 않으므로, **보안에 유리하다 (중간에서 값을 가로채서 바꿀 수 없다)**.
- String 값이 변경되지 못하므로, **Thread-safe하다**.
  - 내부 데이터가 변하지 않으므로 여러 스레드가 같은 String에 동시 접근하더라도 데이터가 변하지 않는다.

<br>

> 참고로, Java 7까지 String Pool은 Heap 내부의 Permanent영역에 존재하였는데, 
>
> Permanent영역은 공간이 정적으로 할당되어, String Pool의 크기가 늘어나면 OOM이 발생할 소지가 있었다.
>
> 따라서  Java 8부터는 String Pool 을 Heap으로 옮겨 다이내믹한 메모리 위에서 동작하게 되었다고 한다.
>
> 또한, Heap 위에 있으므로 Garbage Collection의 대상이 되므로 메모리 관리가 효율적이다.

<br>

String은 객체다.

따라서 String 연산은 **새로운 String 객체를 String Pool에 생성하므로,**

그에 따른 오버헤드를 발생시킬 수 있다.

아래 코드를 보자.

```java
public static void main(String[] args) {
    
    String a = "Test1";
    String b = "Test1";
    String c = "Test2";
    String d = a + b;
    
    System.out.println("a의 Hash값 : " + System.identityHashCode(a));
    System.out.println("b의 Hash값 : " + System.identityHashCode(b));
    System.out.println("c의 Hash값 : " + System.identityHashCode(c));
    System.out.println("a + b의 Hash값 : " + System.identityHashCode(d));
}
```

```
a의 Hash값 : 1313922862
b의 Hash값 : 1313922862
c의 Hash값 : 122883338
a + b의 Hash값 : 666641942
```

Hash값은 주소값은 아니지만, String은 출력 시 toString() 을 출력하도록 재정의되어있으므로

객체의 고유한 hashCode를 리턴하는 identityHashCode 메소드를 사용하였다.

위 과정을 순차적으로 살펴보면, 

- String a가 "Test1" 로 초기화될 때, String Pool에 "Test1" 값을 가진 객체가 생성된다.
- String b가선언될 때 이미 Pool에 "Test1"이 생성되어 있으므로, 같은 주소값을 가리킨다.
- String c가 "Test2"로 초기화돨 때, 마찬가지로 Pool에 새 객체가 생성된다.
- String d가 생성될 때, a + b 값은 Pool에 새로운 객체를 만들고, 다른 주소값을 가진다.

즉, 새로운 리터럴이 생성될 때마다 객체가 새로 생성되므로,

이에 대한 오버헤드가 발생되기 마련이다.

그럼 코딩을 하다 String 연산을 해야하는 경우는 어떻게 처리해야 할까????

<br>

---

#### StringBuilder와 StringBuffer

StringBuilder와 StringBuffer는 둘 다 String처럼 문자열을 관리하지만,

String과는 다르게 **Mutable(가변)**하다는 특징을 지닌다.

둘 다 마찬가지로 String과 같이 char[]로 이루어진 문자열 저장 객체이지만,

String이 한 번 char[]가 정해지면 변하지 않는(final)것과 달리,

StringBuilder와 StringBuffer는 **append()**함수로 **byte[]배열에 계속 추가적인 값을 할당**한다.

즉, **StringBuilder와 StringBuffer의 문자열은 동적이다.**

배열의 특성 상 초기화 시 배열의 크기가 정해진다.

예를 들어 Java에서 StringBuilder 클래스의 생성자와 상속받는 추상클래스는 아래와 같다.

```java
/**
 * Creates an AbstractStringBuilder of the specified capacity.
 */
AbstractStringBuilder(int capacity) {
    if (COMPACT_STRINGS) {
        value = new byte[capacity];
        coder = LATIN1;
    } else {
        value = StringUTF16.newBytesFor(capacity);
        coder = UTF16;
    }
}

/* 위는 StringBuilder의 추상클래스로, StringBuilder 클래스가 상속한다 */

/**
* Constructs a string builder with no characters in it and an
* initial capacity of 16 characters.
*/
@HotSpotIntrinsicCandidate
public StringBuilder() {
    super(16);
}
```

**initial capacity of 16 characters** , 즉 처음에는 capacity 16으로 byte[] value 필드를 초기화하는데,

만약 계속되는 append()로 인해 길이가 16을 넘어버린다면,

기존 배열을 복사한 후 필요한 만큼 길이가 늘어난 배열에 다시 value를 넣고 반한한다.

흔히 StringBuilder나 StringBuffer를 쓸 때는 값을 전부 append하거나 remove한 후, 

toString() 메소드로 최종적으로 문자열 객체를 만든다.

**그렇기에 당연히 매번 String 객체를 생성하는 것보다 성능이 좋다.**

따라서, **다수의 문자열 연산이 필요한 경우 StringBuffer와 StringBuilder를 쓰는 것이 좋다.**

<br>

#### 두 클래스의 차이점

아래 코드를 보면,

```java
@Override
@HotSpotIntrinsicCandidate
public synchronized StringBuffer append(String str) { // StringBuffer
    toStringCache = null;
    super.append(str);
    return this;
}
```

```java
@Override
@HotSpotIntrinsicCandidate
public StringBuilder append(String str) { // StringBuilder
    super.append(str);
    return this;
}
```

StringBuffer는 **synchronized**키워드가 붙어있는 것을 알 수 있다.

따라서 StringBuffer는 **동기화**가 가능하기 때문에 **Thread-safe**하다는 장점이 있다.

두 객체 모두 Mutable하므로, 멀티스레드 환경에서는 원하는 output값이 변하지 않도록 StringBuffer를 써야 한다.

단, **synchronized**는 성능 저하(다른 스레드가 block되므로)를 불러오는 키워드이기 때문에, 

싱글스레드 환경에서는 StringBuilder가 낫다.



