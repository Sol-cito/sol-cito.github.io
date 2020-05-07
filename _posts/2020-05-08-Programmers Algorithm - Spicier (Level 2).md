---
layout: post
title: "프로그래머스 힙(Heap) - 더 맵게 (Level 2)"
subtitle: Algorithm Solution
background: '/img/bg_technology.jpg'
categories: technology
---

###### 문제 설명 

매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다. 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.

```
섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)
```

Leo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다.
Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때, 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요.

##### 제한 사항

- scoville의 길이는 2 이상 1,000,000 이하입니다.
- K는 0 이상 1,000,000,000 이하입니다.
- scoville의 원소는 각각 0 이상 1,000,000 이하입니다.
- 모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return 합니다.

##### 입출력 예

| scoville             | K    | return |
| -------------------- | ---- | ------ |
| [1, 2, 3, 9, 10, 12] | 7    | 2      |

##### 입출력 예 설명

1. 스코빌 지수가 1인 음식과 2인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.
   새로운 음식의 스코빌 지수 = 1 + (2 * 2) = 5
   가진 음식의 스코빌 지수 = [5, 3, 9, 10, 12]
2. 스코빌 지수가 3인 음식과 5인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.
   새로운 음식의 스코빌 지수 = 3 + (5 * 2) = 13
   가진 음식의 스코빌 지수 = [13, 9, 10, 12]

모든 음식의 스코빌 지수가 7 이상이 되었고 이때 섞은 횟수는 2회입니다.



출처 : https://programmers.co.kr/learn/courses/30/lessons/42626



---



#### My solution (Java)

```java
import java.util.PriorityQueue;

class Solution {
    public int solution(int[] scoville, int K) {
        int answer = -1;
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for (int i = 0; i < scoville.length; i++) {
            pq.add(scoville[i]);
        }
        int count = 0;
        while (pq.size() > 1) {
            int newScoville = pq.poll() + (pq.poll() * 2);
            pq.add(newScoville);
            count++;
            if (pq.peek() >= K) {
                answer = count;
                break;
            }
        }
        return answer;
    }
}
```



---

#### My logic & Feedback

이 문제는 Priority Queue를 이용하지 않으면 효율성을 통과할 수 없는 문제다.

주어진 수의 범위 및 크기가 너무 크기 때문에,

'**가장 스코빌 지수가 낮은 음식**'을 찾기 위해 매번 탐색 or 정렬을 한다면 N^2의 시간복잡도가 나오기 때문이다.

따라서 힙 트리구조를 이용하여 저절로 정렬을 하게 해주는 Priority Queue를 사용하여 시간복잡도를 LogN으로 줄여아만 한다.

사실 본 문제를 두 달쯤 전에 풀이를 시도하였으나...당시는 힙 구조 및 Priority Queue를 몰랐기 때문에 당연히 풀지 못했다.

그러나 알고리즘 스터디를 통해 Priority Queue의 쓰임을 익혔고, 연습문제로 골라 풀었더니 15분만에(...) 답이 나와서 당황했다.

Priority Queue는 자바 내장함수이긴 하지만, Heap tree 구조를 모르고 쓴다면 그 근본 원리를 모르고 쓰는 것이기 때문에 겉핥기가 될 것 같다.

따라서 이번주에는 Queue 및 Heap Tree 자료구조를 직접 구현하여 우선순위 큐의 원리에 대해 학습해볼 계획이다.



