---
layout: post
title: "[Java] Java 8 - 모던 자바의 특징"
subtitle: "Lambda, Stream, Default & static interface, etc.."
background: '/img/bg_technology.jpg'
categories: technology-javaandpython
---

많은 기술면접에서 Java 8에 대한 질문이 들어오곤 한다.

자바는 2021년 4월 현재 버전이 16까지 발표되었는데,

왜 Java 8을 강조하는 것일까?

그것은 **Java 8 에서부터 자바에 '모던 프로그래밍'의 특징들이 추가되었기 때문**이다.

(또한 아직 실무에서 Java 8 을 많이 쓰고 있기 때문이라 한다...)

시대가 흐르면서 하드웨어의 성능 또한 지속적으로 향상되었다.

CPU 또한 멀티코어가 일반화 될 정도로 계속 진보해왔는데,

멀티코어로 변화된 하드웨어 환경은 효율적인**'병렬처리'**에 대한 고민을 개발자들에게 던졌다.

> 병렬처리란?
>
> 멀티코어(멀티프로세서)로 하나의 작업을 분산 처리하도록 하여 처리 성능을 높이는 것.
>
> CPU의 '분할정복' 기법이라 할 수 있다.
>
> 단순히 CPU 코어가 여러개라 하여 병렬처리가 되는 것이 아니라,
>
> 반도체 기술, 운영체제, 프로그래밍 언어, 컴파일러 등 베이스 환경이 잘 받쳐주어야 한다.
>
> 코어가 여러개인만큼, 그 활용에 있어서 복잡도 또한 엄청나게 증가하기 때문이다.

Java진영에서도 이러한 멀티코어 환경에 따라 그에맞는 **새로운 패러다임**을 제시해야 하였고,

그 결과 추가된 여러가지 기능이 Java 8 에 담겨있다.

물론 그와 더불어 유용한 개선점 또한 포함되어있다.

즉, Java 8 은 **모던 Java의 시초**이므로 굉장히 중요하다.

---

#### Feature 1 - Lambda

람다식의 의미는 자바가 **함수형 프로그래밍**을 지원하기 시작했다는 것이다.

함수형 프로그래밍은 **대규모 병렬처리**를 쉽게 만들어준다. 어떻게?

함수형 프로그래밍은 기본적으로 **순수함수를 통한 불변성(Immutability)**을 지향한다.

순수함수는 내부 상태를 갖지 않아, 동일한 입력에는 항상 동일한 출력이 보장되기 때문이다.

상태변화가 발생하지 않으므로 부작용이 없으며, 따라서 대규모 병렬처리에 장점을 지닌다.

(병렬처리 프로그래밍을 해보지 않아서 감은 오는데 정확히 피부로 와닿진 않음)

자바8 의 람다식은 완전한 함수형 프로그래밍을 지향하는 것은 아니다.

기본적으로 람다 메소드는 **인터페이스**를 통해 호출되므로(@FunctionalInterface 필요)

객체의 형식을 요구하기 때문이다.

그럼에도 불구, **코드가 간결해지고, 가독성이 향상되며, 병렬처리에 용이하다**는 장점이 있다.

반면, **무명함수의 재사용 불가, 까다로운 디버깅, 재귀에 부적합**하다는 단점도 지닌다.

<br/>

#### Feature 2 - Stream API

컬렉션의 각 element를 조작하는 연산을 할 때, 

Java에서는 보통 for문으로 각 요소에 접근해왔다(스트림을 몰랐던 나도 그래왔따..).

Java 8 부터는 스트림과 람다를 통해 **컬렉션 data 각 요소에 쉽게 접근**할 수 있으며,

**parallelStream 를 통해 멀티스레드 코드 없이도 데이터를 병렬처리할 수 있다.**

아래 코드를 보자.

```java
int[] arr = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

int count = 0;
for (int i = 0; i < arr.length; i++) {
    if (arr[i] > 5) {
        count++;
    }
}
System.out.println(count);
```

특정 배열의 요소 중 5보다 큰 element를 필터링하는 for문이다.

이걸 stream과 filter를 사용하여 표현하면..

```java
int[] arr = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

IntStream stream = Arrays.stream(arr); // 배열 스트림 생성

long count = stream.filter(x -> x > 5).count(); // count() 결과는 long을 반환

System.out.println(count);
```

이렇게 for문과 중간 연산 filter, 람다를 활용하여 컬렉션 반복을 멋지게 표현할 수 있다.

(이 외에도 map, reduce, sort 등 다양한 활용방법이 있다)

Stream은 이렇듯 **간결한 코드로, 다양한 함수를 조립(like 함수형 프로그래밍)**한다는 장점을 지닌다.

또한 **병렬화 성능**을 높일 수 있다는 장점이 있다.

```java
Stream<Integer> listNormalStrm = list.stream(); // 일반 스트림
Stream<Integer> listParallelStrm = list.parallelStream(); // 병렬 스트림
```

위 두 개의 스트림은 하나는 일반, 하나는 **멀티 쓰레드** 방식으로 연산을 처리하는 병렬 스트림이다.

병렬 스트림 사용 시 프로그램이 알아서 **ForkJoinFramework**를 수행하여 병렬 처리를 한다.

(개발자가 쓰레드 풀을 생성할 필요 없이 내부적으로 알아서 쓰레드 풀 생성함)

```java
public class Main {
	public static void main(String[] args) {
		List<Integer> list = new ArrayList<Integer>();
		for (int i = 0; i < 30000000; i++) {
			list.add(i);
		}

		Stream<Integer> listNormalStrm = list.stream();
		long startTime = System.currentTimeMillis();
		listNormalStrm.filter(x -> x % 2 == 0).count();
		long endTime = System.currentTimeMillis();
		System.out.println("일반 스트림 소요시간 : " + (endTime - startTime));

		Stream<Integer> listParallelStrm = list.parallelStream();
		startTime = System.currentTimeMillis();
		listParallelStrm.filter(x -> x % 2 == 0).count();
		endTime = System.currentTimeMillis();
		System.out.println("병렬 스트림 소요시간 : " + (endTime - startTime));
	}
}

/* 결과 */
일반 스트림 소요시간 : 109
병렬 스트림 소요시간 : 48
```

filter를 활용하여 3천만개 배열 element 중 짝수를 filtering하여 count() 하기까지의 소요시간을 비교한 코드다.

병렬 스트림이 약 2배 높은 성능을 보여줌을 알 수 있다.

**그러나, 항상 병렬 스트림이 성능이 좋은 것은 아니다. 자칫하면 장애를 일으킬 수도 있다.**

관련해서는 [이 블로그](https://multifrontgarden.tistory.com/254) 를 참고해보자.

<br/>

#### Feature 3 - default, static 메서드가 추가된 Interface

기존에는 인터페이스에 메서드를 추가하게 되면 

해당 인터페이스를 구현하는 모든 클래스에도 해당 메서드를 구현해야만 했다.

그러나 Java 8 에서 추가된 default method를 활용하면,

**이전에 개발한 구현체를 변경없이 그대로 사용하면서,**

**새롭게 개발하는 클래스에서는 default method를 활용할 수 있다.**

가령,

```java
public interface Test {
	public abstract void Test1(); // 기존 추상 메서드

	public default void Test2() { // 디폴트 메서드 - 함수 내용 추가 가능
		System.out.println("test");
	}
}
```

```java
/* 기존 클래스 */
public class TestInstance implements Test{

	@Override
	public void Test1() {
		// TODO Auto-generated method stub
		
	}
}

/* 새로운 클래스 */
public class TestInstance2 implements Test {

	@Override
	public void Test1() {
		// TODO Auto-generated method stub
	}

	@Override
	public void Test2() {
		// TODO Auto-generated method stub
	}
}

```

위와 같이 기존 클래스는 default 메소드 추가 없이 사용 가능하며,

새로 만드는 클래스에서만 Test2() 메서드를 구현해주면 된다. 물론 Override도 가능하다.

즉, **디폴트 메서드는 기존 인터페이스에 안전하게 추가하여 유연성을 높여주는 효과를 지닌다**.

<br/>

**정적(static) 메서드**란, 객체 없이 인터페이스만으로도 호출이 가능한 메서드다.

```java
public interface Test {
	public static void staticMethod() {
		System.out.println("정적 메소드 호출");
	}
}
```

위와 같이 정적메서드를 구현하면,

```java
public class Main {
	public static void main(String[] args) {
		Test.staticMethod();
	}
}
```

이렇게 구현체 없이 바로 호출이 가능하다.

default 메서드와 static 메서드 둘 다 Java 8 이 제공하는 **유연한 프로그래밍을 위한 기능**이다.

<br/>

#### Feature 4 - 새로운 시간 API : java.time

자바8 이전의 Calendar, Date 클래스는 여러가지 문제점을 지니고 있었다.

윤달이 표현되지 않는다거나, 불변 객체가 아니어서 값이 변동한다거나 하는 문제가 있었는데,

Java 8 부터는 기존에 존재하던 **Joda-Time** 이라는 좋은 라이브러리를 참고하여 만든 시간 패키지를 제공한다.

**LocalDate, LocalTime, LocalDateTime, ZonedDataTime** 등 유용한 시간 관련 클래스를 제공한다.

복잡한건 없으니 직접 써보면서 익히면 좋을 것 같다.

<br/>

#### 그 외 특징들

- Hash Collision 발생 시 기존에는 LinkedList를 활용하여 체이닝하였으나, Java 8에서부터는 LinkedList의 크기가 8 이상이 되면 **TreeMap**으로 버킷 자료구조를 바꾼다.


- **Nashorn(나즈혼)**이라는 향상된 자바스크립트 기본 엔진을 제공한다 (그 전까지는 Rhino 활용)


- 기타등등..

<br/>

모던 자바의 특징을 잘 익히면 간결하면서도 유연하고, 보기에도 멋진 코드를 작성할 수 있을 것 같다!

