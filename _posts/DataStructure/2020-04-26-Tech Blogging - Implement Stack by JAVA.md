---
layout: post
title: "JAVA로 Stack 구현해보기"
subtitle: "ArrayList 와 Pointer로 push, pop, isEmpty, peek, clear 구현"
background: '/img/bg_technology.jpg'
categories: technology-datastructure
---



---

### 자료구조의 기초를 튼튼히 하자

알고리즘 문제를 풀면서 Java에 내장되어있는 ArrayList, Stack, Queue 등의 자료구조를 써왔지만, 그런 자료구조들을 직접 구현해본적은 없다.

수학을 배울 때도 공식 유도를 무조건 해보고 문제풀이로 넘어가듯이, 자료구조를 나름대로 직접 구현해봐야 한다는 생각은 예전부터 하고 있었다.

알고리즘 스터디를 진행하면서 우선순위 큐, 힙 등 새로운 유형의 문제들을 계속 접하게 되었고, 아무래도 직접 자료구조들을 구현해봐야 이를 응용한 다른 자료구조들을 익힐 수 있다고 느꼈기에, 천천히 하나씩 직접 구현해보고자 한다 (자료구조 책도 한 권 샀다.).

가장 먼저 구현해 볼 녀석은 Stack이다.

---

### Stack의 원리

Stack은 '컴퓨터 구조' 프로젝트를 진행하면서 이미 한 차례 훑으며 공부해본 적이 있는데,

왜냐하면 컴퓨터 프로그램이 실행될 때, 변수 및 메소드의 실행 역시 '**후입선출**'이라는 Stack의 계층구조 원리로 이루어지기 떄문이다.

Java로 예를 들면, 

우선 Stack의 맨 위에 main() 메소드가 push 된다. 

그리고 main 메소드가 부르는 다른 메소드가 스택 위에 차례로 push된다.

그리고 그 메소드들이 부르는 또 다른 메소드들이 계속 push되고,

호출이 끝나면 stack 최상단에서부터 아래로 순차적으로 메소드가 실행된다.

Java Intellij나 Eclipse를 실행하다가 에러가 나면 에러메시지가 콘솔창에 뜨는데, 에러가 난 부분을 최상단에 보여주고, 그 아래에 해당 메소드를 부른 다른 메소드들이 차례차례 나타난다.

Stack 구조 위에 메소드들이 쌓여있다가 순차적으로 에러가 나기 때문에 에러가 난 놈부터 먼저 보여주고, 그 아래 하단에 쌓여있는 것들이 pop되면서 보여지는 것이다.

원리는 단순하나, 이 단순한 원리로 다양한 연산을 구현할 수 있다.

실제로 현재 Stack으로 컴퓨터 구조 프로젝트의 Virtual Machine 코드를  분석하는 중이다.



---

### 구현

제네릭 클래스로 CustomizedStack을 만든다. 

멤버변수로는 index를 가리킬 **pointer(초기값 0)**와 Element를 담을 **ArrayList**가 있다.

포인트는, ArrayList에 있는 Element 데이터들이 실제로 삭제되거나 하는 것이 아니라, 모든 메소드의 실행은 '**pointer가 현재 가리키는 index**' 를 기준으로 한다는 것이다.

우리는 ArrayList에 무엇이 얼마나 담겨있는지를 신경쓸 것이 아니라, pointer의 위치만 신경쓰면 된다.

물론 이렇게 ArrayList로 구현하면 쓸데없는 메모리 사용이 생기므로, 사실은 LinkedList로 구현하는 것이 더 적합해보인다. 

pop을 했을 때 연결관계를 끊어버리면 가비지 콜렉터가 알아서 정리해줄테니까..하지만 아직 LinkedList 구현을 해보지 않았으므로 ArrayList로 구현했다.

LinkedList도 내가 산 교재에 구현하는 내용이 있으니, 이 내용은 추후에 적용시켜보자.



1. **push**() 
   - Element를 stack에 push한다.
   - ArrayList에 Element를 add하고, pointer를 1 증가시킨다.
2. **pop**()
   - Element를 꺼낸다.
   - pointer를 1 감소시키고, pointer가 가리키고 있는 Element를 return한다. 
   - 꺼낼 것이 아무것도 없다면(isEmpty) EmptyStackException를 던진다.
3. **isEmpty**()
   - pointer가 0을 가리키고 있다면 true, 아니면 false를 반환한다.
4. **peek**() 
   - pointer가 가리키고 있는 Element를 return한다 (pop과 달리 pointer가 움직이지는 않는다).
   - peek할 것이 아무것도 없다면(isEmpty) EmptyStackException를 던진다.
5. **clear**() 
   - pointer를 0으로 만든다.



---

### Source Code



```java
class CustomizedStack<E> {
    private ArrayList<E> arrayList;
    private int pointer;

    public CustomizedStack() {
        arrayList = new ArrayList<>();
        pointer = 0;
    }

    public void push(E element) {
        arrayList.add(element);
        pointer++;
    }

    public E pop() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        E element = arrayList.get(--pointer);
        return element;
    }

    public boolean isEmpty() {
        if (pointer == 0) {
            return true;
        }
        return false;
    }

    public E peek() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        return arrayList.get(pointer - 1);
    }

    public void clear() {
        pointer = 0;
    }
}
```



