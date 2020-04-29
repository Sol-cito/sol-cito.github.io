### HashMap은 빠르고 편리하다

알고리즘 문제를 풀 때 Hash를 많이 사용하는 편인데, 

그 이유는 특정 상황에서 Hash가 매우 성능이 좋고 사용이 간편하기 때문이다.

특정 데이터를 List가 아닌 Map에 저장해놓으면, 다시 그 데이터를 찾을 때 **탐색**의 과정을 생략할 수 있기 때문이다.

그래서 HashMap의 자료구조와 원리는 무엇이기에 이렇게 나를 편리하게 만들어주는지

Java를 기준으로 한번 파보기로 했다.



------

#### Hash 의 의미

Hash의 영어 사전적 의미는 '**특정 데이터를 숫자로 표현해 나타내는 것**'이다. 

간단히 말해 '**데이터와 숫자를 Mappin**g하는 것'이라 할 수 있다.

일상생활에서도 이런 예를 흔히 찾아볼 수 있는데, 학번, 사번, 제품번호 등 '특정 데이터에 **고유하게** 부여된 숫자'가 이렇게 Hashed된 것들이다.

HashMap도 마찬가지로 특정 Value를 숫자 Key의 형태로 Mapping해놓은 자료구조다.



#### 배열, HashMap, 그리고 메모리

```
int[] array = new int[10] 
```

위 배열이 있을 때, 

이 배열 또한 'index'라는 Key가 저절로 부여되고, Integer Value를 담고있다고 볼 수도 있으나,

배열의 경우 size가 dynamic하게 증감하지 못하고, 단순히 index로만 값을 잡아야 하니 Key-value pair로 맵핑이 되어있다고 보기 어렵다.

그러나 Hash는 Key값에 Integer, String, Long 등 다양한 변수타입이 들어갈 수 있으며(그렇기에 자바에서는 제네릭 타입으로 선언된다), size도 다이내믹하게 변한다(정확히 말하면 size 자체가 바뀌는건 아니지만 어쨌든...)

배열은 생성당시 크기가 선언되는데, 이 크기대로 메모리에 배열 영역이 잡힌다.

위 배열은 length가 10이므로, 메모리값은 0x0001, 0x0002, 0x0003...과 같이 공간이 할당되는 방식이다.

해시는 조금 다르다.

해시는 Key가 배열처럼 index가 순서대로 부여되는 것이 아니라 사용자가 직접 Key를 지정하는 방식이기 때문에 처음부터 Size가 규정되지 않는다.

그렇다고 해서 Integer의 가능한 범위를 모두 Key의 범위로 잡아버리면, 메모리 낭비가 클 뿐더러, String과 같은 자료형으로 Key를 설정하면 일일히 범위를 잡는 것도 불가능에 가깝다.

따라서 해시는 **Hash Function**및  **Hash Table** 로 새롭게 Key를 부여하여 데이터를 저장해놓는다.

그럼 아래에서 HashMap이 java에서 내부적으로 어떻게 작동하는지 자세히 살펴보자.



---

### Hash Function과 Table

```
HashMap<Integer, String> map = new HashMap<>();
map.put(1,"Kevin");
```

위와 같이 put 메소드로 키값이 1인 Kevin이라는 String value가 들어왔다고 하자.

Hash 내부에서는 이렇게 put으로 들어온 Key, Value 쌍을 **Node<K, V>** 객체로 만들어 저장한다.



```
    static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        Node<K,V> next;
        }
```



4개의 멤버변수 각각의 의미는 다음과 같다.



- int hash : Key의 **hashcode**를 의미한다. 
- K key : put 메소드를 통해 들어온 key
- V value : put 메소드를 통해 들어온 value
- Node<K , V> next : 다음 key-value 쌍을 가리키는 **포인터**



이렇게 만들어진 Node 객체는 **table**이라는 **초기 Length 16**의 배열에 저장된다.

앞서 말했듯이 Key의 범위를 가능한 모든 범위로 잡아버리면 메모리 낭비가 너무 커지므로, table은 초기 크기 16에서 그 범위를 초과할 경우 2배씩 사이즈가 증가한다.

그럼 Node 객체는 table의 몇 번째 index에 저장되는가?

Index를 결정하는 것이 바로 **int hash** 로, 이 값은 **hashCode**라는 함수에 의해 부여된다.

HashCode를 부여하는 방법은 여러가지가 있는데, 가장 직관적인 방법은(개인적으로) '나눗셈법(Division Method)'으로 보인다.

파라미터로 입력받은 Key(K)를 table 배열의 크기(최초크기 16)로 나눈 나머지를 hashCode로 지정하는 방법이다

```
int hash = K % table.length;
```

이 방법은 hash 값이 table의 index 를 벗어나지 않음을 보장하기 때문이다.

(위 방법은 다수의 Node가 table의 특정 index에 중첩되어 저장될 - Hash Collision이 발생할 - 가능성이 있기에 이를 상쇄하기 위한 여러가지 방법이 있다고 한다)

다수의 Node가 동일한 table index를 가질 때 활용되는 것이 마지막 멤버변수 **next**이다.

Java가 Hash Collision을 해결하는 방법은 Chaining으로, 말 그대로 동일한 index를 hash로 한 객체들끼리는 **LinkedList**의 형태로 chain처럼 hash를 공유함으로서 리스트화된다.

이를 그림으로 표현하면 아래와 같다 (그림에서는 Node객체가 Entry로 기술됨).



![(How HashMap Works Internally In Java)](https://i0.wp.com/javaconceptoftheday.com/wp-content/uploads/2016/02/HashMapInternalStructure.png?w=1200)

(Source : https://javaconceptoftheday.com/how-hashmap-works-internally-in-java/)



찾는 값이 처음 table index의 첫 번째 chain에 없으면 다음번 chain의 Node 주소를 보고, 그래도 없으면 그 다음번 chain의 Node 주소를 보고..하는 방식이다.

이 때문에 특정 index에 Node가 몰려서 저장되어 버리면 또 다른 탐색이 이루어지기 때문에, index별로 적절히 Node를 분배하는 것이 성능향상에 중요하다.



---

### 자료구조의 중요성

HashMap은 JDK1.2 버전부터 제공되었다고 한다.

JDK1.2는 1998년 12월에 release되었으니, 그 전에는 Hash 구조를 직접 구현해서 쓰거나 그냥 List를 탐색했었을 것이다.

Hash도 하나의 자료구조이다. 

이 자료구조로 인해 특정 값 탐색 성능이 엄청나게 향상되었을 것이고,

지금은 나같은 개발 입문자도 편하게 쓸 수 있을 정도로 많은 영향을 미치고 있다.

IT업계의 많은 개발자들이 '**자료구조와 알고리즘 학습의 중요성**'을 매번 강조하는 이유를 새삼 느낀다.