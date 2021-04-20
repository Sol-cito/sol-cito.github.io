---
layout: post
title: "[Java] Collection Framework"
subtitle: "Map, List, Set"
background: '/img/bg_technology.jpg'
categories: technology-javaandpython
---

Java에는 프로그래머를 위해 **컬렉션 프레임워크**라고 하는 다양한 자료구조 묶음을 제공한다.

완성된 자료구조의 제공으로 프로그래머들의 **자료구조 구현의 수고를 덜어주고,**

최적화된 알고리즘을 제공하여 **high performance**를 보장한다.

또한 Framework라는 말대로 표준화된 interface의 제공으로 설계에 **확장성을 부여**한다.

자 그러면, Java의 Collection Framework의 **구조**와 **구현체**, **인터페이스**를 살펴보도록 하자.

---

#### Collection Framwork 자료구조의 종류

백문이 불여일견, 아래 그림을 보자.

![image](https://www.ict.social/images/1/java/collections/java_collection_api_diagram.svg)

(출처 : [https://www.ict.social/java/collections-and-streams-in-java/java-collections-framework](https://www.ict.social/java/collections-and-streams-in-java/java-collections-framework))

<br/>

뭔가 복잡해 보이지만, 구조를 찬찬히 뜯어보자.

Bottom-up 으로 맨 아래 박스부터 살펴보면,

**빨간색 박스**는 **컬렉션 프레임워크 추상 클래스를 상속한 클래스**, 즉 **실제 프로그래머가 사용하는 자료구조 클래스**이다.

그 위 **초록색 박스**는 해당 자료구조 클래스가 상속하는 **추상클래스**이다.

<br/>

추상 클래스의 종류를 살펴보면 자료구조의 종류를 크게 **네 가지**로 나눌 수 있음을 알게 된다. 정리하면 아래와 같다.

| AbstractSet   | AbstractList | AbstractQueue | AbstractMap   |
| ------------- | ------------ | ------------- | ------------- |
| HashSet       | ArrayList    | PriorityQueue | HashMap       |
| LinkedHashSet | LinkedList   |               | LinkedHashMap |
| TreeSet       | Vector       |               | TreeMap       |

>  LinkedList같은 경우 배열이 아니라 Node형태로 자료를 저장하므로 AbstractSequentialList를 상속하지만,
>
> AbstractSequentialList도 결국 AbstractList를 상속한다.

또한, 해당 추상 클래스들의 **구체적인 행동(method)**을 제공해주는 **Interface** 또한 정의되어 있다.

Interface는 다중 implementation이 가능하므로, 여러 Interface가 구현되어있는 자료구조가 대부분이다.

**Set, List, Queue**의 Interface의 공통조상은 **Collection Interface**와 **Iterable Interface**라고 할 수 있다.

내부 코드를 살펴보면, 

**Collection Interface**는 add(), remove(), contains()와 같은 구체적인 행위 메소드를 명시해놓은 반면,

**Iterable Interfcae**는 forEach 와 같은 반복과 관련된 메소드를 제공한다.

따라서 이 두 조상을 가진 subclass인 위 자료구조들은 for-each-loop 및 기본적인 콜렉션 메소드들을 재정의하고있다.

(Iterable 인터페이스에 대해서도 공부해야될 게 한바닥인거 같은데..이건 다음 기회에..)

**Map**은 set, list, queue와는 달리 Collection Interface를 상속하지 않는다.

Map은 **Key / Value**의 형태로 데이터를 저장하는 자료구조이므로, 다른 자료구조들과는 다른 모습과 기능을 지닌다.

그렇지만 컬렉션 프레임워크에 포함되는 자료구조이다.

<br/>

### Each data structure's feature

각각의 자료구조는 그들 나름대로의 고유한 기능과 특징이 있다.

따라서 그 원리와 특징을 간략하게나마 정리하고자 한다.

<br/>

#### 1. AbstractList를 상속하는 List 자료구조

- **ArrayList**

  - ```java
    private static final int DEFAULT_CAPACITY = 10;
    ```

  - 보다시피 **기본 배열값 16**으로 시작하여 크기가 늘어나면 **기존 배열을 복사한 후 더 큰 배열에 옮긴다.**

  - Dynamic Array라고 보면 된다. 단, 배열 복사의 오버헤드가 발생한다.

  - 마찬가지 이유로 **데이터의 삽입, 삭제 시 또 배열복사를 해야하므로 추가 오버헤드가 발생한다.**

  - **Thread-safe하지 않다.**

    ```
    his class is roughly equivalent to
     * {@code Vector}, except that it is unsynchronized.
    ```

- **LinkedList**

  - 배열에 자료를 저장하는 것이 아니라 **Node**에 저장하는 자료구조.
  - Head, Tail로 이어져 있는 Doubly 구조이며, 메모리 상에서 흩어져 있으므로**ArrayList보다 캐시 히트가 낮다.**
  - 마찬가지 이유로 index로 탐색 시 **ArrayList보다 느리다.**
  - 다만 **데이터의 삽입, 삭제 시 포인터만 변경하면 되므로 오버헤드가 없다.**

- **Vector**

  - 사실 ArrayList는 **향상된 Vector**라고 할 수 있으며, 따라서 나 또한 Vector는 한 번도 사용해 본 적이 없다.

  - 자바에서도 아래와 같이 권유한다.

  ```
    * implementation is not needed, it is recommended to use {@link
    * ArrayList} in place of {@code Vector}.
  ```

  - 다만, **Vector는 ArrayList에서 찾아볼 수 없는 synchronized 키워드가 있다 --> Thread-safe하다.**

<br/>

#### 2. AbstractMap을 상속하는 Map 자료구조

- **HashMap**

  - 대표적인 Map 자료구조. 
  - Key, Value를 이용하여 자료를 저장하므로 **순서의 개념이 없다.**
  - Key, Value를 저장하는 **Node**객체는 **table**이라는 크기 16의 배열에 저장된다(연관배열). 저장될 때의 **hashCode 값**에 따라 그 index가 결정된다. ==> 이와 같은 이유로 **순서가 보장되지 않는 것**
  - HashCollision 발생을 방지하기 위해 **LinkedList chaining**을 활용하며, LinkedList의 크기가 8이 넘으면 **TreeMap**으로 전환한다.
  - [HashMap 과거 포스트 참고](https://sol-cito.github.io/technology-datastructure/2020/04/29/Tech-Blogging-HashMap)

- **LinkedHashMap**

  - **순서를 보장하는** 해시맵. HashMap을 상속한 자식 클래스다.
  - Map에 put으로 특정 node객체가 만들어질 때 마다 **그 node객체를 Doubly LinkedList로 chaining한다.**
  - 즉 또 다른 노드를 만들어 LinkedList 형태로 계속 이어붙이는 방식으로 순서를 만드는 것.
  - 또 다른 링크드 리스트가 필요하므로 **추가 메모리 소모**가 필요하다.

- **TreeMap**

  - **자동 정렬**을 구현한 **레드 블랙 트리**(비편향 트리)로 이루어져있다.
  - 아래 예시를 보면 Entry와 Key가 각각 오름차순 정렬되어 반환됨을 알 수 있다 (내림차순도 가능).

  ```java
  public class Main {
      public static void main(String[] args) {
          TreeMap<String, String> treeMap = new TreeMap<>();
          /* 알파벳 순서 무작위로 put */
          treeMap.put("z", "z value");
          treeMap.put("f", "f value");
          treeMap.put("e", "e value");
          treeMap.put("a", "a value");
          treeMap.put("i", "i value");
  
          Map.Entry<String, String> firstEntry = treeMap.firstEntry();
          String firstKey = treeMap.firstKey();
          System.out.println(firstEntry); // 첫 번째 Entry
          System.out.println(firstKey); // 첫 번째 Key
  
          Map.Entry<String, String> lastEntry = treeMap.lastEntry();
          String lastKey = treeMap.lastKey();
          System.out.println(lastEntry); // 마지막 Entry
          System.out.println(lastKey); // 마지막 key
      }
  }
  
  [출력 결과 : a, z가 출력된다 - 오름차순 정렬됨]
  a=a value
  a
  z=z value
  z
  ```

<br/>

#### 3. AbstractSet을 상속하는 Set 자료구조

- **HashSet**
  - 기본적인 집합 자료구조.
  - how? : 필드변수에 **HashMap**을 선언하여, add시 현재 map에 Key값으로 add하려는 element가 **존재하는지** 확인한다.
    - map의 put() 메소드는 키값이 중복되면 value를, 아니면 null을 return하기 때문에 중복 판단이 가능하다.
  - 존재하지 않으면 add, 아니면 리턴한다.
  - map을 사용하므로, **자료를 add한 순서대로 저장되리라는 보장이 없다**.
  - 왜? map은 hashCode로 HashTable을 구성하므로, hashCode를 도출하여 index를 어떻게 만드느냐에 따라서 자료의 출력 순서가 뒤바뀔 수 있기 때문이다.
  - HashMap에 int값을 1부터 100까지 차례대로 저장한 후 하나씩 출력하면 1부터 100까지가 차례대로 출력되지만(hashCode값을 도출하기 위해 %연산을 이용하므로 숫자는 괜찮은 듯 하다), String을 저장하면 출력 순서가 제멋대로다.
- **LinkedHashSet**
  - LinkedHashMap과 마찬가지로 **Doubly LinkedList**를 활용하여 **순서가 보장되는** HashSet이다.
- **TreeSet**
  - TreeMap과 마찬가지로 **자동 정렬**을 구현한 **레드 블랙 트리**(비편향 트리)로 이루어져있다.

<br/>

#### 4. AbstractQueue를 상속하는 Priority Queue 자료구조

우선순위 큐...는 Heap구조에 대한 포스팅을 자세히 하고 난 후 함께 다루어볼 예정이니,
이 포스팅에서는 패스하기로 하자.

<br/>