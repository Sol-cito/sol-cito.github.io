---
layout: post
title: "프로그래머스 다이내믹 프로그래밍 - N으로 표현(Level 3)"
subtitle: Algorithm Solution
background: '/img/bg_technology.jpg'
categories: technology-algorithm

---

###### 문제 설명

아래와 같이 5와 사칙연산만으로 12를 표현할 수 있습니다.

12 = 5 + 5 + (5 / 5) + (5 / 5)
12 = 55 / 5 + 5 / 5
12 = (55 + 5) / 5

5를 사용한 횟수는 각각 6,5,4 입니다. 그리고 이중 가장 작은 경우는 4입니다.
이처럼 숫자 N과 number가 주어질 때, N과 사칙연산만 사용해서 표현 할 수 있는 방법 중 N 사용횟수의 최솟값을 return 하도록 solution 함수를 작성하세요.

##### 제한사항

- N은 1 이상 9 이하입니다.
- number는 1 이상 32,000 이하입니다.
- 수식에는 괄호와 사칙연산만 가능하며 나누기 연산에서 나머지는 무시합니다.
- 최솟값이 8보다 크면 -1을 return 합니다.

##### 입출력 예

| N    | number | return |
| ---- | ------ | ------ |
| 5    | 12     | 4      |
| 2    | 11     | 3      |

##### 입출력 예 설명

예제 #1
문제에 나온 예와 같습니다.

예제 #2
`11 = 22 / 2`와 같이 2를 3번만 사용하여 표현할 수 있습니다.

출처 : https://programmers.co.kr/learn/courses/30/lessons/42895



---



#### My solution (Java)

```java
import java.util.ArrayList;
import java.util.HashMap;

class Solution {
     public int solution(int N, int number) {
        int answer = 1;
        if (N == number) { //5를 이용한 횟수가 1번이면 바로 return
            return answer;
        }
        /* 각 케이스 별 결과 list를 담을 HashMap */
        HashMap<Integer, ArrayList<Integer>> map = new HashMap<>();
        ArrayList<Integer> case_1 = new ArrayList<>();
        case_1.add(N);
        map.put(1, case_1);
        /* answer를 -1로 만들고 시작 */
        answer = -1;
        for (int i = 2; i <= 8; i++) { //number가 사용되는 수를 2부터 8까지 탐색
            int leftNumber = 1;
            int rightNumber = i - 1;
            while (leftNumber < i) {
                /* left, right의 수는 각각 N을 몇번 이용했는가 이므로 조합의 경우의 수를 활용하면 됨. */
                ArrayList<Integer> caseResult = new ArrayList<>();

                ArrayList<Integer> leftList = map.get(leftNumber);
                ArrayList<Integer> rightList = map.get(rightNumber);

                for (int a = 0; a < leftList.size(); a++) {
                    int fromA = leftList.get(a);
                    for (int b = 0; b < rightList.size(); b++) {
                        int fromB = rightList.get(b);
                        /* 덧셈 */
                        int plus = fromA + fromB;
                        caseResult.add(plus);
                        /* 뺼셈 */
                        int minus = fromA - fromB;
                        caseResult.add(minus);
                        /* 나누기 */
                        int division = 0;
                        if (fromB != 0) {
                            division = fromA / fromB;
                            caseResult.add(division);
                        }
                        /* 곱하기 */
                        int multiply = fromA * fromB;
                        caseResult.add(multiply);
                        /* 붙이기 */
                        int add = fromA * 10 + fromB;
                        caseResult.add(add);
                        if (plus == number || minus == number || division == number || multiply == number || add == number) {
                            answer = i;
                            return answer;
                        }
                    }
                }
                map.put(i, caseResult);
                leftNumber++;
                rightNumber--;
            }
        }
        return answer;
    }
}
```



---

#### My logic & Feedback

하...

이 문제는 스터디 숙제로 나왔던 문제였으나 결국 풀지못하고,

3주가 지나서야 다시 시도해서 풀었던 문제다.

다이내믹 프로그래밍 문제의 정석과도 같은 문제라 할 수 있는데, 다이내믹 프로그래밍이 무엇인지 그 개념이 없던 나로서는 도저히 당시에 알던 지식만으로는 풀 수가 없었다.

그러다 정말 신기하게도, '컴퓨터 구조'를 공부하다가 갑자기 이 문제의 풀이 방법이 떠올랐다.

당시 나는 가상머신 언어를 어셈블리어로 바꾸는 Project를 하고있었는데, 해당 Project는 가상머신 언어로 '피보나치 수열'을 구현하고 n번째 피보나치 수가 무엇인지 알아내는 스택 연산을 구현하는 것이었다.

피보나치 수열이 무엇인지는 당연히 알고있었는데, 피보나치 수열을 '거꾸로' 생각해본적은 없었다.

늘상 1 1 2 3 5 8 13 .... 이런 식으로 작은 수부터 순차적으로 피보나치 수열을 생각해왔지,

'13이라는 수는 피보나치의 몇 번째 수인가?' 와 같은 문제는 접해보지 못했던 것이다.

어쨌든 그렇게 피보나치 수열을 보던 중, '어? 이 방법으로 그 때 못풀었던 문제를 풀 수 있겠는데?' 하는 생각이 번뜩 스쳤다.

그래서 구글에서 다이내믹 프로그래밍의 정보를 검색해보니, 역시 피보나치 수열과 마찬가지로 

**'N번째 수는 N-1번째와 N-2번째의 연산 결과다'**라는 원리를 갖고있다는 것을 깨달았다.

다이내믹 프로그래밍의 굉장히 유명한 예제는 '통나무 자르기' 문제이다. 이 예제를 이해한다면 위 N으로 표현 문제도 같은 원리로 쉽게 풀 수 있다.

위 문제 풀이의 핵심은,

"number는 a와 b의 연산 결과이며, a와 b는 y개의 N의 연산 결과이다" 라는 것이다.

예를 들어 12 = 55 / 5 + 5 / 5 의 경우,

12는 {[5를 3번 연산한 결과값] 과 [5를 2번 연산한 결과값]의 연산 결과} 이다.

즉 5를 총 5번 연산한 결과인데, 이 5번은 5를 3번 + 5를 2번 연산한 결과로 나타낼 수 있다.

5를 3번 연산한 결과는 다시 {[5를 2번 연산한 결과]와 [5를 1번 연산한 결과]의 연산결과} 로 나타낼 수 있다.

이런 식으로 5를 1번~8번까지 연산한 결과값을 모두 저장해 두고, 조합해가면서 최소 연산값을 구해내면 된다.

특정 결과에서 거꾸로 내려가는 식으로 접근한다는 사고가 있어야만 해답을 찾을 수 있는 문제다.

이 문제는 정말로 풀어내고 싶었는데, 그 이유는 아예 접근방법조차 감이 오지 않아 자존심이 약간 상했기 때문이다.

그러나 결국 이렇게 유레카 moment를 거쳐서 해답을 발견해내니, 이렇게 기쁠 수가 없다.

