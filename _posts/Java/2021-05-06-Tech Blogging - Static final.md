| layout | title                        | subtitle            | background             | categories               |
| ------ | ---------------------------- | ------------------- | ---------------------- | ------------------------ |
| post   | [Java] Static과 final 키워드 | Constant, immutable | /img/bg_technology.jpg | technology-javaandpython |

```java
public static final int OK = 1;
```

위 코드는 Java에서 상수를 만들 때 일반적으로 사용하는 방법이다.

**static**과 **final**키워드를 이용하여 상수를 만드는데..

왜 static과 final을 굳이 같이 쓰는걸까?

<br/>

#### static 이란

static은 해당 키워드가 붙은 데이터, 메소드, 클래스가 '정적'이라는 의미이며,

public 으로 선언되었다면 프로그램 전역에서 이들을 호출하는 것이 가능하다.

왜냐하면 static 키워드가 붙은 것들은 모두 **컴파일 시 JVM의 Method Area**에 위치되기 때문이다.

static은 클래스 내 변수, 메소드 둘 다에 붙일 수 있으며,

각각의 경우 instance 생성 없이도 **전역변수**, **전역메소드**로 기능한다.

대표적인 static method는 **public static void main(String[] args)** 이다...

```java
public class Test {
	public static void main(String[] args) {
		System.out.println(StaticTest.x); // static 변수 값 0
		StaticTest.printStatic(); // "static test"를 print하는 메소드
	}
}

class StaticTest {
	static int x; // static 변수. 초기화하지 않았으므로 0으로 기본값이 들어감

	public static void printStatic() { // static 메소드
		System.out.println("static test");
	}
}
```

위 코드에서 StaticTest 클래스는 x라는 static 변수와 printStatic() 메소드를 가지며,

두 변수 및 메소드는 컴퍼일 시 JVM의 Method Area에 기록된다.

<br/>

#### final 이란

final 키워드가 붙은 변수, 메소드, 클래스는 각각 '상수화'의 특징을 담는다.

변수가 아닌 상수는 **변하지 않는 것**의 의미를 가지므로,

변수는 값이 고정되고, 메소드도 행동이 고정되고, 클래스도 형태가 고정된다.

즉, **할당이 딱 한번만 된다**는 뜻이다.

(final 자체가 '상수로 만든다'라는 의미라기 보다는, **할당이 딱 한번만 가능하다**는 의미로 받아들이는 것이 좋겠다)

선언과 동시에 할당되거나, 선언만 해 놓고 나중에 딱 한번만 할당되거나...

메소드와 클래스의 오버라이드, 상속은 **다형성**을 의미하므로,

변하지 않는다는 상수의 의미와 배치된다.

그렇기에 값의 변동, 오버라이드, 상속이 금지되는 것이다.

> private 메소드 또한 암묵적으로 final의 의미를 가진다.
>
> 상속 시 자식클래스에서 접근할 수 없고, 따라서 오버라이드가 불가하기 때문이다.

<br/>

**1) 변수** - 상수화

```java
public class Test {
	public static void main(String[] args) {
		FinalTest finalTest = new FinalTest(10); // 상수 값 할당
		finalTest.x = 100; // 이미 값이 할당되었으므로 에러 발생
	}
}

final class FinalTest {
	final int x; // 상수 선언

	public FinalTest(int x) {
		this.x = x; // 생성 시 상수 할당됨
	}
}
```

**2) 메소드** - 오버라이드 금지됨

```java
class FinalOverrideTest {
	public final void printMethod() {
		System.out.println("Can't override");
	}
}

class FinalOverrideTest_child extends FinalOverrideTest {
	@Override
	public void printMethod() { // 에러 발생
	}
}
```

**3) 클래스** - 상속 금지됨

```
final class FinalTest {
}

class FinalTest_child extends FinalTest{ // 에러 발생
}
```

<br/>

> static 없이 final 키워드만 활용하는 경우(인스턴스 생성 시 한 번만 초기화 하고 변동을 요하지 않는 경우) -> 대표적으로 Spring Framework의 DI(의존성 주입)의 경우가 있다.

<br/>

#### 그래서 static final 이란?

그러니까 결국 static final 키워드가 같이 붙는다는 의미는...

**컴파일 단계에서 메모리에 잡힐 불변값을 선언**한다는 의미이다.

즉, 이 값은 컴파일 시 이미 **초기화**가 되어야만 메모리에 올라가므로,

**선언과 동시에 초기화되어야만 한다.**

```java
class FinalTest {
	static final int value1 = 10;
	static final int value2; // 에러 발생
}
```

어떤 프로그램에서 모든 클래스 전역에 쓰이는 특정 변수가 있을 때 이렇게 static final을 사용한다.

가령 어떤 학교 학생들의 정보를 다루는 프로그램이 있을 때,

모든 학생은 **같은 학교**에 소속되어 있으므로,

학교 이름을 **static final**로 한다면 해당 상수를 프로그램 전역에서 활용 가능할 것이다.

<br/>

자, 그럼 Java에서 상수값은 static, final과 같은 키워드로만 다루어야 할까?

그에 대해서는 **Enum** 포스트에서 살펴보도록 하겠다.