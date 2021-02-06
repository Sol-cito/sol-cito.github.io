---
layout: post
title: "프로그래머스 야근 지수 (Level 3) "
subtitle: Algorithm Solution
background: '/img/bg_technology.jpg'
categories: technology/tech-algorithm
---

###### 문제 설명

회사원 Demi는 가끔은 야근을 하는데요, 야근을 하면 야근 피로도가 쌓입니다. 야근 피로도는 야근을 시작한 시점에서 남은 일의 작업량을 제곱하여 더한 값입니다. Demi는 N시간 동안 야근 피로도를 최소화하도록 일할 겁니다.Demi가 1시간 동안 작업량 1만큼을 처리할 수 있다고 할 때, 퇴근까지 남은 N 시간과 각 일에 대한 작업량 works에 대해 야근 피로도를 최소화한 값을 리턴하는 함수 solution을 완성해주세요.

##### 제한 사항

- `works`는 길이 1 이상, 20,000 이하인 배열입니다.
- `works`의 원소는 50000 이하인 자연수입니다.
- `n`은 1,000,000 이하인 자연수입니다.

##### 입출력 예

| works     | n    | result |
| --------- | ---- | ------ |
| [4, 3, 3] | 4    | 12     |
| [2, 1, 2] | 1    | 6      |
| [1,1]     | 3    | 0      |

##### 입출력 예 설명

입출력 예 #1
n=4 일 때, 남은 일의 작업량이 [4, 3, 3] 이라면 야근 지수를 최소화하기 위해 4시간동안 일을 한 결과는 [2, 2, 2]입니다. 이 때 야근 지수는 22 + 22 + 22 = 12 입니다.

입출력 예 #2
n=1일 때, 남은 일의 작업량이 [2,1,2]라면 야근 지수를 최소화하기 위해 1시간동안 일을 한 결과는 [1,1,2]입니다. 야근지수는 12 + 12 + 22 = 6입니다.

출처 : https://programmers.co.kr/learn/courses/30/lessons/12927



---

#### My first solution (Java)

```java
import java.util.Arrays;
class Solution {
    public long solution(int n, int[] works) {
        long answer = 0;

        while (n != 0 && works[works.length - 1] != 0) {
            Arrays.sort(works);
            int length = works.length;
            if (works[0] == works[length - 1]) { //모든 원소의 크기가 같을 때
                if (n >= length) { //n이 원소 개수 이상일 때
                    int difference = n / length;
                    for (int i = 0; i < length; i++) { //모든 원소에서 diff만큼 빼준다
                        if (works[i] >= difference) {
                            works[i] -= difference;
                            n -= difference;
                        } else { //diff가 원소보다 크면 원소 크기만큼 빼준다.
                            works[i] = 0;
                            n -= works[i];
                        }
                    }
                } else { //아닐 때 - 모든 원소를 n이 0이 될 때까지 1씩 감소시킴
                    for (int i = 0; i < length; i++) {
                        if (n == 0) {
                            break;
                        }
                        works[i] -= 1;
                        n -= 1;
                    }
                }
            } else { //다를 때
                int endPointer = length - 1;
                int startPointer = 0;
                while (works[startPointer] < works[endPointer]) { 
                    //가장 큰 원소집단의 startPoint 잡기
                    startPointer++;
                }
                for (int j = startPointer; j < length; j++) {
                    works[j]--;
                    n--;
                    if (n == 0) {
                        break;
                    }
                }
            }
        }
        long result = 0;
        for (long each : works) {
            result = (long) each * (long) each;
            answer += result;
        }
        System.out.println(answer);
        return answer;
    }
}
```

---

#### My logic & feedback

어렵게 풀려다가 망한 문제...

- `works`는 길이 1 이상, 20,000 이하인 배열입니다.
- `works`의 원소는 50000 이하인 자연수입니다.
- `n`은 1,000,000 이하인 자연수입니다.

라는 제한사항 때문에, 1씩 뺀다면 당연히 시간초과가 날 거라고 생각했다.

그래서, 우선 정렬을 한 후 큰 수 집합들을 만들고, 거기서 두 번째 큰 수들과의 차를 계산하고, 또 빼고...

하는 방법을 구현했는데, 생각보다 너무 복잡해서 구현에 시간이 오래걸렸다.

그리고 결국 스터디 끝에, 가장 큰 수 집합들에서 1씩 뺀다 하더라도 효율성 테스트를 통과함을 알게 되었다.

어렵게 접근하면 한없이 어려워지는 문제였다.

스터디원들은 대부분 우선순위 큐를 이용해서 풀었다.

나는 아직 개똥 자존심 때문에 '내가 직접 구현해보지 않은 우선순위 큐(힙 트리)는 사용하지 않겠다' 라고 생각하고 있는데, 사실 달리 말하면 아직까지 귀찮아서(...) 우선순위 큐를 직접 구현을 '못해본'상태다.

이번 주는 휴일이 많은데, 휴일동안 1. 큐 구현 2. 힙 트리 공부 3. 우선순위 큐 공부

를 알고리즘 & 자료구조 Study의 목표로 잡아야겠다.
