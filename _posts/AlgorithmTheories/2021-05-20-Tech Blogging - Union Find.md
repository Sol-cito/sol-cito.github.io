---
layout: post
title: "[Algorithm] Union find"
subtitle: "Disjoint set"
background: '/img/bg_technology.jpg'
categories: technology-algorithm
---

유니온 파인드 알고리즘은 **집합**에 관한 알고리즘이다.

좀 더 상세히 말하자면, **집합의 구성요소들을 효율적으로 찾고**,

**다른 집합을 효율적으로 합치도록 만들어주는**알고리즘이라 할 수 있다.

<br/>

A = [1,2,3,4,5,....1000]

B = [1001, 1002, 1003, 1004, ...., 2000]

라는 두 집합이 있다고 해 보자.

여기서 아무 두 수를 뽑아서 그 두 수가 **동일한 집합에 속해있는지를 판단(find)**하는 문제가 있다면?

<키 , 값> 의 자료구조(HashMap, Dictionary 등)를 사용하면 쉽게 풀린다.

각 집합의 구성요소들을 O(N)으로 한 번 스캔하면서, 

<element 값, 집합의 이름> 의 구조로 저장해놓으면 

두 element가 같은 집합에 속해있는지 O(1)의 시간복잡도로 판단할 수 있다.

<br/>

하지만 두 집합을 **합친다면(Union)?**

둘 중 하나의 집합 요소를 다시 한번 풀 스캔하여 value값을 합쳐진 집합의 이름에 맞게 모두 수정해주어야 한다.

만약 어떤 문제에서 **합침(Union) - 동일 집합인지 판단(find)**의 질문이 빈번하게 발생(Q번이라 하자)하며,

집합의 크기가 매우 크다면?

위 방법으로는 O(N x Q) 의 시간복잡도가 소요될 것이다.

가령, 아주 대표적인 아래 백준 문제를 보자.

[집합의 표현](https://www.acmicpc.net/problem/1717)

element는 최대 100만개(초기에는 각 element가 단일집합을 구성함),

Union/find 연산은 최대 10만개가 주어진다.

위 방법으로 Union을 연산한다면 Union이 발생할 때 마다 O(집합의 크기) 의 시간복잡도가 발생할 것이고,

Union/Find 연산이 최대 10만개 이므로 어마어마한 복잡도가 발생한다.

이러한 문제를 **트리구조**를 통해 효율적으로 해결하도록 도와주는 **유니온 파인드** 알고리즘을 알아보자. 

<br/>

기본 원리는 다음과 같다.

- 집합은 root 노드로부터 시작하는 트리구조로 표현된다.
- 따라서 두 노드가 **동일한 집합**에 속해있다는 말은, **동일한 root노드를 지닌다**는 의미이다.
- 또한 두 집합을 합치는 작업은 **두 트리가 동일한 root노드를 지닌다**는 것과 같다.

유니온파인드는 **Union함수**와 **find함수**를 각각 구현하여 문제에서 필요한 부분에 활용한다.

> 참고로 union 이라는 word는 예약어로 쓰이는 경우가 있기에, union이 아닌 merge로 표현하기도 한다.

<br/>

#### Find 함수

파이썬으로 find함수를 구현한 결과는 아래와 같다.

```python
def find(parents, num):
    if parents[num] == num: return num # root노드는 항상 자기 자신의 번호를 가리킨다.
    res = find(parents, parents[num])
    parents[num] = res # 경로압축최적화
    return res

parents = [i for i in range(n + 1)] # n개의 노드가 있을 때
```

먼저 parents 배열에 자기 자신을 root로 하는 모든 노드를 설정한다.

그리고 **재귀함수**로 root노드를 찾아가는 find함수를 구현하는데,

여기서 재귀함수의 return값을 **최종 root노드 값**으로 하여

재귀함수가 반환될 때 각 노드의 parents 배열값을 갱신한다.

이를 **경로압축최적화**라고 하며, 이 경우 효율성이 더욱 증가한다.

만약 트리가 다음과 같이 **일렬로 쭉 늘어진** 형태라고 생각해보자.

```
1 - 2 - 3 - 4 - 5 (root)
```

이 경우 find를 할 때 O(N)의 시간복잡도가 든다. 일렬 구조가 변하지 않기 때문이다.

그러나, 어차피 1,2,3,4 의 root는 5로 동일하다면, find를 하면서 아래와 같은 구조로 만들어버리면?

```
   5(root)
/ | | \
1 2 3 4
```

find함수 수행과 동시에 1,2,3,4가 바로 root 노드에 붙어버리기 때문에, 

다음번 find부터는 연산 시간을 크게 줄일 수 있다.

<br/>

#### Union 함수

파이썬으로 Union함수를 구현한 결과는 아래와 같다.

```python
def union(parents, first, second):
    # first, second 노드의 root를 각각 구한다.
    rootA = find(parents, first) 
    rootB = find(parents, second)
    
    # root 노드가 같으면 같은 집합에 속해있다는 의미이므로 바로 return한다.
    if rootA == rootB: return
    
    # root 노드가 다르면 root노드를 어느 한 쪽에 맞춰준다.
    parents[rootB] = rootA
    return
```

결국 find 함수만 잘 구현한다면, union 함수는 구현이 상당히 간단하다.

그 말은, **Union 함수의 효율은 결국 find 함수에 종속된다**는 의미이기도 하다.