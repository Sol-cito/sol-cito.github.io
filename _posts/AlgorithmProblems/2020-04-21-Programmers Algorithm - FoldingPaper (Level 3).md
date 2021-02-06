---
layout: post
title: "프로그래머스 카카오 Winter Coding- 종이접기 (Level 3)"
subtitle: Algorithm Solution
background: '/img/bg_technology.jpg'
categories: technology/tech-algorithm
---

###### 문제 설명

직사각형 종이를 n번 접으려고 합니다. 이때, 항상 오른쪽 절반을 왼쪽으로 접어 나갑니다. 다음은 n = 2인 경우의 예시입니다.



먼저 오른쪽 절반을 왼쪽으로 접습니다.



다시 오른쪽 절반을 왼쪽으로 접습니다.



종이를 모두 접은 후에는 종이를 전부 펼칩니다. 종이를 펼칠 때는 종이를 접은 방법의 역순으로 펼쳐서 처음 놓여있던 때와 같은 상태가 되도록 합니다. 위와 같이 두 번 접은 후 종이를 펼치면 아래 그림과 같이 종이에 접은 흔적이 생기게 됩니다.



위 그림에서 ∨ 모양이 생긴 부분은 점선(0)으로, ∧ 모양이 생긴 부분은 실선(1)으로 표시했습니다.

종이를 접은 횟수 n이 매개변수로 주어질 때, 종이를 절반씩 n번 접은 후 모두 펼쳤을 때 생기는 접힌 부분의 모양을 배열에 담아 return 하도록 solution 함수를 완성해주세요.

##### 제한사항

- 종이를 접는 횟수 n은 1 이상 20 이하의 자연수입니다.
- 종이를 접었다 편 후 생긴 굴곡이 ∨ 모양이면 0, ∧ 모양이면 1로 나타냅니다.
- 가장 왼쪽의 굴곡 모양부터 순서대로 배열에 담아 return 해주세요.

------

##### 입출력 예

| n    | result          |
| ---- | --------------- |
| 1    | [0]             |
| 2    | [0,0,1]         |
| 3    | [0,0,1,0,0,1,1] |

##### 입출력 예 설명

입출력 예 #1
종이의 오른쪽 절반을 왼쪽으로 한번 접었다 펴면 아래 그림과 같이 굴곡이 생깁니다.



따라서 [0]을 return 하면 됩니다.

입출력 예 #2
문제의 예시와 같습니다.

입출력 예 #3
종이를 절반씩 세 번 접은 후 다시 펼치면 아래 그림과 같이 굴곡이 생깁니다.



따라서 [0,0,1,0,0,1,1]을 return 하면 됩니다.



출처 : https://programmers.co.kr/learn/courses/30/lessons/62049



---

#### My first solution (Java)

```java
class Solution {
  public int[] solution(int n) {
        int[] answer = {};
        String leftSide = "0";
        leftSide = recursion(leftSide, 1, n);
        String[] list = leftSide.split("");
        answer = new int[list.length];
        for (int i = 0; i < list.length; i++) {
            answer[i] = Integer.parseInt(list[i]);
        }
        return answer;
    }

    public String recursion(String leftSide, int times, int n) {
        if (times == n) {
            return leftSide;
        }
        StringBuilder sb = new StringBuilder();
        char[] letters = leftSide.toCharArray();
        for (int i = letters.length - 1; i >= 0; i--) {
            if (letters[i] == '0') {
                sb.append('1');
            } else {
                sb.append('0');
            }
        }
        String rightSide = sb.toString();
        return recursion(leftSide + "0" + rightSide, times + 1, n);
    }
}
```

---

#### My logic & feedback

이게 왜 Level 3...?

종이접기의 규칙만 발견하면 바로 해결되는 문제다..

라고 생각했으나 시간초과가 났고, StringBuilder를 이용하니 바로 풀렸다...

String 연산은 왜 느릴까?

다음은 스터디원들이 제공해준 정보다.

```
String 클래스는 문자열 연산이 적고, 자주 참조(조회)하는 경우에 사용하면 좋다.
변하지않는 변수로서 사용하기에는 편리하지만,
연산할 때 내부적으로 계속 인스턴스를 생산하고, charAt 메소드를 쓰므로 느리다. 

반면, 
StringBuffer와 StringBuilder 클래스는 변경이 용이하다. 문자열의 저장및 "변경을 위한 메모리 공간"을 지닌 클래스이다. 둘의 차이는 멀티스레드 환경에서 동기화 차이이다. stringBuffer는 동기화가 가능하여 thread-safe 하고, builder는 그렇지 않지만 좀 더 빠른 성능을 보인다.  

사용 예 ) 
StringBuilder sb = new SringBuilder();
sb.append("ptsd");
StringBuffer stb = new StringBuffer();
stb.append("my");
stb.append("eyes");
stb.insert(1,"poor");

그리고 스트링 배열은 더하기 연산을 할때마다 스트링객체를 만드는데 스트링 객체는 클래스이므로 내부적인 함수들이 그때마다 생성되서 이에 소모되는 시간또한 크다.
```

