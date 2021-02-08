---
layout: post
title: "프로그래머스 가장 긴 팰린드롬 (Level 3)"
subtitle: Algorithm Solution
background: '/img/bg_technology.jpg'
categories: technology-algorithm

---

###### 문제 설명

앞뒤를 뒤집어도 똑같은 문자열을 팰린드롬(palindrome)이라고 합니다.
문자열 s가 주어질 때, s의 부분문자열(Substring)중 가장 긴 팰린드롬의 길이를 return 하는 solution 함수를 완성해 주세요.

예를들면, 문자열 s가 abcdcba이면 7을 return하고 abacde이면 3을 return합니다.

##### 제한사항

- 문자열 s의 길이 : 2,500 이하의 자연수
- 문자열 s는 알파벳 소문자로만 구성

------

##### 입출력 예

| s       | answer |
| ------- | ------ |
| abcdcba | 7      |
| abacde  | 3      |

##### 입출력 예 설명

입출력 예 #1
4번째자리 'd'를 기준으로 문자열 s 전체가 팰린드롬이 되므로 7을 return합니다.

입출력 예 #2
2번째자리 'b'를 기준으로 aba가 팰린드롬이 되므로 3을 return합니다.



출처 : https://programmers.co.kr/learn/courses/30/lessons/12904



---



#### My solution (Java)

```java
class Solution{
     public int solution(String s) {
        int answer = 1;
        StringBuilder sb = new StringBuilder();
        int length = s.length();
        /* sb를 이용 */
        char[] splited = s.toCharArray();
        for (int i = 0; i < length; i++) {
            sb.append(splited[i]);
        }
        int endPointer = 0;
        int startPointer = 0;
        boolean finish = false;
        for (int i = 0; i < length; i++) {
            startPointer = i;
            endPointer = length;
            while (startPointer >= 0 && endPointer - startPointer > 1) {
                answer = verifyPallinrome(sb.substring(startPointer, endPointer));
                if (answer != 0) {
                    finish = true;
                    break;
                }
                startPointer--;
                endPointer--;
            }
            if (finish) {
                break;
            }
        }
        return answer;
    }

    /* 팰린드롬인지 검사하는 메소드 */
    public int verifyPallinrome(String target) {
        int answer = 1;
        char[] charList = target.toCharArray();
        int startIndex = 0;
        int endIndex = charList.length - 1;
        while (startIndex < endIndex && startIndex != endIndex) {
            if (charList[startIndex] == charList[endIndex]) {
                startIndex++;
                endIndex--;
                answer = target.length();
            } else {
                answer = 0;
                break;
            }
        }
        return answer;
    }
}
```



---

#### My logic & Feedback

처음에 subString으로 접근했다가 효율성을 통과하지 못하여, ASCII 코드로 int[] 배열을 만들고 통과하였다.

String은 연산속도가 느리기 때문에, int 비교연산을 통하여 성능을 향상시키고자 하였다.

int[] 배열에 각 문자의 char를 int로 캐스팅 한 것을 넣고, startPointer 와 endPointer로 가장 긴 문자열들 부터 비교해나가면 풀리는 비교적 간단한 문제.