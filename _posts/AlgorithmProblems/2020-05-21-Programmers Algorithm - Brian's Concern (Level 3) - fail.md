---
layout: post
title: "프로그래머스 브라이언의 고민 - 2017 Kakao (Level 3) - 실패"
subtitle: Algorithm Solution
background: '/img/bg_technology.jpg'
categories: technology-algorithm

---

###### 문제 설명

**알림: '실행'을 눌렀을 시 올바른 코드가 틀린 결과로 표시되는 경우가 있습니다. 하단의 설명을 참고해주세요.**

카카오스토리의 개발자 브라이언에게 최근 고민이 생겼다. 하루에도 수백만 명이 사용하는 서비스답게 사람들이 많이 보는 글에 광고성 댓글을 달아 불쾌감을 유발하는 사용자가 증가하고 있는데, 신고를 받은 글이 광고글인지를 운영자가 판단하여 차단하는 시스템으로는 빠르게 늘어나는 광고글을 처리하기 어렵기 때문이다. 그래서 브라이언은 신고된 글이 광고글인지를 자동으로 판단하는 시스템을 만들었다. 이제 사용자가 광고글을 보고 신고하면 그 글이 광고글로 판단된 경우 자동으로 차단된다! 드디어 깨끗한 카카오스토리를 만들었다는 기쁨도 잠시, 광고글을 올리는 사람들이 자동 차단 시스템을 회피할 수 있는 방법을 찾기 시작했고, 얼마 지나지 않아 광고 문구 사이에 특수문자를 넣으면 차단되지 않는다는 점이 알려지게 되었다. 즉, 아래와 같은 식으로 작성하면 광고글 차단이 적용되지 않는다.

```
♚프☆렌☆즈☆레☆이☆싱♚★사전예약★진행중
$지금$예약시♜이모티콘♜100%※증정※
★라이언★카트♨전원♨획@득@기@회
즉시이동 http://...
```

생각지 못한 광고글 패턴에 당황하던 브라이언은 광고글이 일정한 규칙에 의해 만들어진다는 사실을 알게 되었는데, 그 규칙은 다음과 같다.
(아래 설명 및 그 이후의 내용에서 영문 대문자는 원래 문구, 소문자는 특수기호를 의미한다.)

- 광고글은 원래 문구에 다음 규칙을 적용하여 만들 수 있다.
  - (규칙 1) 특정 단어를 선택하여 글자 사이마다 같은 기호를 넣는다. ex) `HELLO` -> `HaEaLaLaO`
  - (규칙 2) 특정 단어를 선택하여 단어 앞뒤에 같은 기호를 넣는다. ex) `WORLD` -> `bWORLDb`
  - 위의 두 가지 규칙은 한 단어에 모두 적용될 수 있지만 같은 규칙은 두 번 적용될 수 없다.
  - 한 번 쓰인 소문자(특수기호)는 다시 쓰일 수 없다.
- 마지막으로 원래 문구에 있던 공백을 제거한다.

위의 규칙에 따라, `HELLO WORLD`는 다음의 광고 문구로 변환될 수 있다.

- `HELLOWORLD` (기호 삽입 없이 마지막 규칙인 공백 제거만 적용되었다.)
- `HaEaLaLaObWORLDb` (첫 번째 단어에는 규칙 1이, 두 번째 단어에는 규칙 2가 적용되었다.)
- `aHbEbLbLbOacWdOdRdLdDc` (모든 단어에 모든 규칙이 적용되었다.)

단, 아래의 문구는 올바르게 변환된 광고문구가 아니다.

- `aHELLOa bWORLDb` (공백이 제거되어야 한다.)
- `HaEaLaLObWORLDb` (규칙 1은 단어의 모든 글자 사이에 적용되어야 한다. 단, 이 문장은 원문이 `HELL O WORLD`인 경우 올바른 변환이다.)
- `aHELLOWORLDa` (규칙 2는 한 단어에 적용되어야 한다. 단, 이 문장은 원문이 `HELLOWORLD`인 경우 올바른 변환이다.)
- `HaEaLaLaOWaOaRaLaD` (첫 번째 단어에 쓰인 기호 `a`를 두 번째 단어에 쓸 수 없다.)
- `abHELLObaWORLD` (하나의 규칙을 같은 단어에 두 번 적용할 수 없다.)

신고된 글에 대해 위 규칙이 적용되기 전 문구를 찾을 수 있으면 자동 차단 시스템을 다시 온전하게 실행할 수 있게 된다. 카카오스토리가 광고글 없는 깨끗한 공간이 될 수 있도록 프로그램을 만들어보자.

### 입력 형식

입력은 문자열 변수 `sentence`로 주어진다. 이 문자열은 영문 대소문자로만 이루어져 있으며, 길이는 `1,000` 이하이다.

### 출력 형식

입력으로 주어진 광고 문구의 규칙 적용 전 원래 문구를 리턴한다. 단 원래 문구의 경우 문장 앞뒤의 공백이 없어야 하며, 단어 사이의 공백은 한 글자여야 한다. 가능한 답이 여러 가지인 경우 그중 하나를 리턴하면 된다. 규칙에 따른 변환으로 만들 수 없는 문자열이 입력된 경우에는 소문자로 `invalid`를 리턴한다.

### 예제 입출력

| sentence         | answer      |
| ---------------- | ----------- |
| HaEaLaLaObWORLDb | HELLO WORLD |
| SpIpGpOpNpGJqOqA | SIGONG JOA  |
| AxAxAxAoBoBoB    | invalid     |

### 예제에 대한 설명

첫 번째 테스트 케이스는 문제 설명에 제시된 데이터와 같다.
두 번째 테스트 케이스에서, 기호 `q`는 규칙 1, 혹은 규칙 2에 의해 추가된 기호일 수 있다. 규칙 1에 해당하는 경우 원문은 `SIGONG JOA`로 예제 출력과 같으며, 규칙 2에 해당하는 경우의 원문인 `SIGONG J O A`도 올바른 답이다.
세 번째 테스트 케이스에서 `x`는 규칙 1에 의해 추가된 기호여야 한다. (규칙 2에 의해 추가되었다면 기호가 단어 앞뒤에 붙게 되므로 2개여야 한다.) 그러므로 `AAAA`가 한 단어여야 한다. 마찬가지로 `o`도 규칙 1에 의해 추가된 기호여야 하기 때문에 `ABBB`가 한 단어여야 한다. 이는 동시에 만족할 수 없는 조건이므로 주어진 문구는 규칙을 만족할 수 없게 된다. 따라서 `invalid`를 리턴한다.

### 알림

**'실행'을 눌러 기본 테스트케이스에 대해 결과를 확인할 때 정답이 여러 개인 경우에도 하나의 답만 정답으로 처리되고 있습니다. 두 번째 예제 입력에 대해 `SIGONG J O A`를 리턴하는 코드의 경우 정답이지만 올바르지 않은 결과로 표시됩니다. '코드 채점'을 눌러 제출할 시에는 올바르게 채점되니 참고하여 주시기 바랍니다.**

출처 : https://programmers.co.kr/learn/courses/30/lessons/1830



---



#### My solution (Java)

```java
import java.util.ArrayList;
import java.util.HashMap;

class Solution{
   String rule1_Result = "";
    public String solution(String sentence) {
        /* (int)char 에서 A=65, Z=90, a=97, z=122, 공백=32 */
        String answer = "";
        char[] charList = sentence.toCharArray();
        ArrayList<Character> lowerCaseList = new ArrayList<>();
        HashMap<Character, Integer> lowerCaseNumberMap = new HashMap<>();
        for (int i = 0; i < charList.length; i++) {
            char target = charList[i];
            if (target > 96) {
                if (lowerCaseNumberMap.get(target) == null) {
                    lowerCaseList.add(target);
                    lowerCaseNumberMap.put(target, 1);
                } else {
                    int num = lowerCaseNumberMap.get(target);
                    lowerCaseNumberMap.replace(target, num + 1);
                }
            }
        }

        ArrayList<Character> rule1List = new ArrayList<>(); // 규칙 1번에 해당되는 소문자 모두 다 담음
        for (int i = 0; i < lowerCaseList.size(); i++) {
            char target = lowerCaseList.get(i);
            if (lowerCaseNumberMap.get(target) != 2) {
                rule1List.add(target);
            }
        }
        recursion(sentence, rule1List, 0);
        char[] charListForRule2 = rule1_Result.toCharArray();
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < charListForRule2.length; i++) {
            if (charListForRule2[i] < 91) {
                sb.append(charListForRule2[i]);
            } else {
                sb.append(" ");
            }
        }
        rule1_Result = sb.toString();
        char[] lastCharList = rule1_Result.toCharArray();
        sb = new StringBuilder();
        char previous = lastCharList[0];
        sb.append(previous);
        char current = ' ';
        for (int i = 1; i < lastCharList.length; i++) {
            current = lastCharList[i];
            if (current == previous && current == 32) {
                previous = current;
                continue;
            }
            sb.append(current);
            previous = current;
        }
        answer = sb.toString().trim();
        System.out.println(answer);
        return answer;
    }

    public void recursion(String sentence, ArrayList<Character> rule1List, int index) {
        if (index >= rule1List.size()) {
            rule1_Result = sentence;
            return;
        }
        char lowerCase = rule1List.get(index);
        StringBuilder sb = new StringBuilder();
        char[] list = sentence.toCharArray();
        int pointer = 0;
        int lowerCaseLastIndex = 0;
        for (int i = 0; i < list.length; i++) {
            if (list[i] == lowerCase) {
                lowerCaseLastIndex = i;
            }
        }
        pointer = 0;
        while (pointer < list.length) {
            char target = list[pointer];
            if (target != lowerCase) {
                sb.append(target);
            }
            if (pointer == lowerCaseLastIndex + 1) {
                sb.append(" ");
            }
            pointer++;
        }
        String newBuiltSentence = sb.toString();
        recursion(newBuiltSentence, rule1List, index + 1);
    }
}

```



---

#### My logic & Feedback

테스트케이스가 딱 하나밖에 없는 문제인데,

문제풀이에 실패했다..

Invalid를 구현하지 않았으나, 테스트케이스가 Invalid를 요구하는것 같지는 않아서 우선 완성된 부분만 돌려보았으나 테스트케이스 실패..

그런데 이 문제 자체가 규칙1, 2를 적용시킬 수 있는 경우의 수가 많아, 어떤것이 확실한 정답이 될 지 모른다.

문제만 봤을 땐 쉬워보였으나, 

문제푸는데 하루종일 걸린(거의 12시간정도 푼 것 같다) 악마의 문제..

아직 푸는데는 실패했지만, 기록용으로 우선 이렇게 남겨둔다.