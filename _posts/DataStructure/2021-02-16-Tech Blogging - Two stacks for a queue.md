---
layout: post
title: "[Data Structure] Stack 2개로 Queue 구현하기"
subtitle: "역방향 x 2 = 정방향"
background: '/img/bg_technology.jpg'
categories: technology-datastructure
---


오랜만에 자료구조 포스팅이다.

쉽고 재미있는 스택과 큐 응용 문제(인터뷰에서 가끔 물어본다 함...)를 풀어보자.

<br>

스택 2개로 큐를 구현하는 방법은, 

**스택은 자료를 아래에서부터 위로 거꾸로 넣는 자료구조이므로,**

**거꾸로 + 거꾸로 = 정방향** 이라는 원리를 이용하면 쉽다.

<br>

스택 A, B 두 개가 있다고 해 보자.

자료를 넣을 때(push)는 스택 A에 차곡차곡 쌓는다 (데이터가 거꾸로 담김).

자료를 꺼낼 때 (pop)는 A에 거꾸로 쌓인 데이터를 전체를 그대로 B에 넣으면 정방향으로 데이터가 담긴다.

다음 번 pop부터는 B에 데이터가 남아있다면, 그 데이터는 정방향 정렬이 된 것일 테니 바로 pop하면 되고,

B에 데이터가 없다면 A에 쌓여있는 전체 자료를 다시 전부 B로 옮겨오면 된다.

즉, 데이터의 pop과 push의 기능으로 스택을 분리하여 구현하면 간단히 해결된다.

<br>

##### 구현 코드(Java)

```java
import java.util.EmptyStackException;
import java.util.Stack;

public class QueueByTwoStack {
    private Stack stack_A;
    private Stack stack_B;

    public QueueByTwoStack() {
        stack_A = new Stack();
        stack_B = new Stack();
    }

    public void push(Object o) {
        stack_A.push(o);
    }

    public Object pop() throws EmptyStackException {
        if (stack_B.isEmpty()) {
            while (!stack_A.isEmpty()) {
                stack_B.push(stack_A.pop());
            }
        }
        return stack_B.pop();
    }
	
    /* 
     * 엿보기(peek) 의 경우는 pop한 element를 반환하되,
     * pop한 element를 다시 B의 최상단에 push해주면 된다.
     */
    public Object peek() {
        Object poped = pop();
        stack_B.push(poped);
        return poped;
    }
}

```

<br>

#### 왜 이런짓을 하는가?

잘 구현된 built-in 큐를 쓰면 되는데, 왜 굳이 stack 2개로 queue를 구현하는가?

FIFO, LIFO의 자료구조를 잘 이해하고 응용하는 데 써먹을 수도 있다.

stackoverflow의 [한 게시글](https://stackoverflow.com/questions/7395400/why-do-we-do-implement-a-queue-using-2-stacks)에 따르면, 

Haskell, ML 등의 컴퓨터 언어에서는 Queue의 enqueue 함수는 O(n)의 시간이 소요되므로 

(built-in list - singly, forward-linked lists 의 특징에 의해)

O(1)의 시간이 소요되는 Stack 2개를 활용하는 것이 훨씬 효율적이라 한다.

Java는 Stack은 vector, Queue는 LinkedList를 상속하므로 이에 해당되진 않겠지만...

<br>

