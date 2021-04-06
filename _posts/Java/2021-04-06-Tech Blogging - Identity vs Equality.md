---
layout: post
title: "[Java] 동일성 vs 동등성"
subtitle: "Identity vs Equality, 그리고 HashCode"
background: '/img/bg_technology.jpg'
categories: technology-javaandpython
---

#### 기본(원시)타입 자료형과 참조 자료형

Java에서는 8가지 기본 자료형이 있다.

- 참/거짓 : booelan
- 정수 : byte, short, int, long
- 실수 : double, float
- 문자 : char (사실 char도 문자라기 보다는 문자에 맞는 숫자값이다)

뜬금없이 왜 원시 타입 자료형을 정리했을까?

자바에서의 **동일비교 ''=="**는 바로 이 원시인 녀셕들에게만 적용되기 때문이다.

<br/>

#### 동일성(Same identity)

동일하다는 말은 말 그대로 똑같다는 뜻이다.

수학에서의 = 이라 생각하면 된다.

가령,

```java
public class Practice {
    public static void main(String[] args) {
        int a = 100;
        int b = 101;

        double c = 2.13;
        double d = 2.13;

        System.out.println(a == b);
        System.out.println(c == d);
    }
}
```

위 코드의 콘솔 출력 결과는 당연히 false, true 가 나올 것이다.

여기까지만 보면 왜 이런 당연한 말을 이렇게 장황하게 하고있나 할 것이다.

<br/>

그럼 아래를 보자.

```java
public class Practice {
    public static void main(String[] args) {
        int[] arr1 = {1};
        int[] arr2 = {1};

        System.out.println(arr1 == arr2);
    }
}
```

**위 결과는 false다.**

분명 배열이라는 자료형도 같고, 내부값도 1로 같은데 왜 false가 나올까?

그 이유는, 사실 저기 위에서 동일 비교의 대상인 arr1, arr2 라는 변수는 **참조변수**로, 

int[] 객체의 **주소값**을 가지고 있는 변수이기 때문이다.

JVM이 Heap에 있는 객체를 찾아갈 때는 해당 객체의 주소가 무엇인지 알아내야 하고,

해당 객체의 주소는 Stack의 참조변수에 기록되어있다.

Stack영역의 참조변수를 스택포인터가 가리키면 참조변수가 지닌 주소로 이동하여 객체를 가리킬 수 있다.

따라서 위 arr1, arr2 라는 변수는 **다른 객체를 가리키고 있으므로, 그 둘의 주소도 다를 수 밖에 없다.**

메모리 주소값은 보통 16진수로 표현되는데, 위 arr1, arr2의 주소값은 다음과 같다.

```java
public class Practice {
    public static void main(String[] args) {
        int[] arr1 = {1};
        int[] arr2 = {1};
        System.out.println(arr1);
        System.out.println(arr2);
    }
}

[I@723279cf
[I@10f87f48
```

두 객체의 주소값은 각각 16진수로 **723279cf , 10f87f48** 이므로,  == 의 동일 비교가 성립하지 않음을 알 수 있다.

<br/>

만일 다른 변수를 선언하여 동일한 객체를 가리키게 만든다면? 

가리키는 주소값이 동일(==)하므로 결과는 true가 될 것이다.

temp 변수를 선언하여 arr2 객체를 가리키도록 해보자.

```java
public class Practice {
    public static void main(String[] args) {
        int[] arr1 = {1};
        int[] arr2 = {1};

        int[] temp = arr2; // 새 참조변수 선언하여 arr2를 가리키게 한다.

        System.out.println(arr2 == temp);
        System.out.println(arr2);
        System.out.println(temp);
    }
}

true
[I@723279cf
[I@723279cf
```

보다시피 결과는 true, 주소값은 동일함을 알 수 있다.

<br/>

16진수로 보면 헷갈리니까..**hashCode()** 메소드를 써보자.

이 메소드는 주소값을 10진수로 편하게 보여주는 메소드다.

(그 뿐만 아니라 무시무시한 역할을 하는데 그건 잠시후 아래에서..)

결과는,

```java
public class Practice {
    public static void main(String[] args) {
        int[] arr1 = {1};
        int[] arr2 = {1};
        System.out.println(arr1.hashCode());
        System.out.println(arr2.hashCode());
    }
}

1915910607
284720968
```

위와 같이 10진수 hashCode가 출력됨을 알 수 있다.
(16진수 723279cf를 10진수로 만든 결과는 1915910607이다)

<br/>

#### 동등성(Equality)

위에서 계속 언급했던 것은 원시 자료형에 해당되는 '**수의 같음**'이었다.

그럼 **객체 내용의 같음**은 어떻게 비교할까?

Java에서는 **equals()** 함수를 활용하여 동등성을 비교한다.

그런데 잠깐, Java 객체 최상위 클래스인 **Object**클래스에 정의된 equals()메소드는 아래와 같다.

```java
public boolean equals(Object obj) {
	return (this == obj);
}
```

보다시피 **==**기호를 활용한 **동일성 비교**를 기본으로 하고있다.

이러면 결국 equals() 메소드를 쓰더라도 동일성 비교만 할 수 있는 것 아닌가?

**따라서 개발자는 각 클래스의 동등성 비교를 위해 equals() 메소드를 Overriding 해주어야 한다.**

**기본적으로 Object 클래스에서 정의된 equals() 메소드는 '동일성'비교를 하고있기 때문이다**.

<br/>

equals() 메소드를 오버라이딩한 가장 대표적인 클래스가 바로 **String** 클래스다.

자바 소스를 까보면...(Java 13을 기준으로 한 코드다)

```java
public boolean equals(Object anObject) {
	if (this == anObject) { // 주소값이 동일하면 값 또한 동등하므로 바로 true 리턴
		return true;
	}
	if (anObject instanceof String) {
		String aString = (String)anObject;
		if (!COMPACT_STRINGS || this.coder == aString.coder) { // 특정 final 조건을 만족할 시
			return StringLatin1.equals(value, aString.value); // StringLatin1의 equals() 호출
		}
	}
	return false;
}


/* StringLatin1 클래스의 equals() 메소드 */
@HotSpotIntrinsicCandidate
public static boolean equals(byte[] value, byte[] other) {
    if (value.length == other.length) {
        for (int i = 0; i < value.length; i++) { // 두 String의 byte[] 배열을 하나씩 for문으로 비교한다
            if (value[i] != other[i]) {
                return false;
            }
        }
        return true;
    }
    return false;
}
```

위와같이 equals() 메소드가 재정의 되어있음을 알 수 있는데,

결국 **주소값이 동일하거나, String 객체이면서 두 byte[]배열의 길이가 같고 모든 element가 같으면 true** 라는 논리이다.

이처럼 equals()를 재정의함으로서 **객체의 동등 비교**를 직접 구현할 수 있다.

<br/>

#### hashCode() 오버라이딩 - Both or Neither

이렇게 equals()를 재정의하면...아무 문제가 없을까?

**개발자는 equals()를 Overriding할 때 hashCode()도 함께 Overriding해야 하는 의무가 있다.**

그렇지 않으면 HashCode를 활용하는 Java의 자료구조(HashMap, HashTable...)의 이용에 큰 차질이 생길 수 있다.

Java의 HashMap은 Key, Value를 이용하여 값을 저장하는 대표적인 자료구조로,

[HashMap에 관한 포스팅](https://sol-cito.github.io/technology-datastructure/2020/04/29/Tech-Blogging-HashMap) 은 예전에 작성한 적이 있다.

아무튼, HashMap의 get 메소드를 한번 살펴보면,

```java
public V get(Object key) {
    Node<K,V> e;
	return (e = getNode(hash(key), key)) == null ? null : e.value; // hash(key)로 getNode의 키를 넣음
}

/* HashMap 클래스에 정의된 hash() 메소드 */
static final int hash(Object key) {
	int h;
	return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16); // key.hashCode()로 검증
}
```

즉,  HashMap 자료구조는 Key값으로 Node를 get할 때 

앞에서 잠깐 살펴본 **Object 클래스의 hashCode()**를 이용하여 Node를 get해옴을 알 수 있다.

문제는, **equals()를 오버라이딩하여 두 객체가 동등하다면, hashCode 값도 항상 동일해야만 한다**는 점이다.

만일 그렇지 않다면,

예를 들어 HashMap의 Key로 어떤 객체를 사용할 때, 내용이 같은 객체(동등성을 가진)를 생성하여 Key로 넣더라도

**HashCode값이 다르기 때문에 원하는 Value를 가져오지 못하기 때문이다.**

혹은 HashSet 자료구조에 동등한 객체를 add할 때, 동등성이 보장된다면 중복 삽입이 없어야 하지만,

마찬가지로 hashCode를 사용하는 특성 상 **hashCode가 다르지만 내용은 같은 중복 객체**가 add될 수도 있다.

따라서 equals()와 hashCode() 의 오버라이딩은 **Both or Neither**해야만 한다.

<br/>

참고로 **hashCode가 같다고 하여 equals()가 보장되는 것은 아니다**.

HashMap의 **Hash Collision**을 떠올려보자.

HashTable에 Key가 들어갈 때 HashCode메소드를 활용한다.

그런데 테이블의 크기는 한정적이기 때문에, 해시 충돌이 일어날 경우 **linkedList로 Key Bucket**을 만든다.

(java8 부터 버킷 데이터가 일정 개수 이상일 때는 Red-black tree 활용)

즉, **동일한 hashCode key를 가지는 객체들이 존재하며, 객체 탐색은 equals()로 이루어진다**.

<br/>

> hashCode값 찾기 ---> hashCode Bucket 도착 ---> equals()로 get할 객체 찾기

<br/>

정리하자면

**1. 동일성(==)은 '값'의 같음, 동등성(equals())은 '내용'의 같음이다.**

**2. Object 클래스의 equals()는 기본적으로 '동일비교'이므로, 개발자는 동등비교를 위해 equals()를 오버라이딩한다.**

**3. hashCode를 활용하는 자료구조를 문제없이 사용하기 위해, equals()와 함께 hashCode() 또한 오버라이딩한다.**


