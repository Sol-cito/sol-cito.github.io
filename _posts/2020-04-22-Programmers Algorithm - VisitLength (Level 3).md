---
layout: post
title: "프로그래머스 카카오 Winter Coding- 방문길이 (Level 3)"
subtitle: Algorithm Solution
background: '/img/bg_technology.jpg'
categories: technology
---
###### 문제 설명

게임 캐릭터를 4가지 명령어를 통해 움직이려 합니다. 명령어는 다음과 같습니다.

- U: 위쪽으로 한 칸 가기
- D: 아래쪽으로 한 칸 가기
- R: 오른쪽으로 한 칸 가기
- L: 왼쪽으로 한 칸 가기

캐릭터는 좌표평면의 (0, 0) 위치에서 시작합니다. 좌표평면의 경계는 왼쪽 위(-5, 5), 왼쪽 아래(-5, -5), 오른쪽 위(5, 5), 오른쪽 아래(5, -5)로 이루어져 있습니다.

![image](https://res.cloudinary.com/jistring93/image/upload/v1495542181/%EB%B0%A9%EB%AC%B8%EA%B8%B8%EC%9D%B41_qpp9l3.png)

예를 들어, ULURRDLLU로 명령했다면

![image](https://res.cloudinary.com/jistring93/image/upload/v1495542443/%EB%B0%A9%EB%AC%B8%EA%B8%B8%EC%9D%B42_lezmdo.png)

- 1번 명령어부터 7번 명령어까지 다음과 같이 움직입니다.

![image](https://res.cloudinary.com/jistring93/image/upload/v1495542704/%EB%B0%A9%EB%AC%B8%EA%B8%B8%EC%9D%B43_sootjd.png)

- 8번 명령어부터 9번 명령어까지 다음과 같이 움직입니다.

![image](https://res.cloudinary.com/jistring93/image/upload/v1495542767/%EB%B0%A9%EB%AC%B8%EA%B8%B8%EC%9D%B44_hlpiej.png)

이때, 우리는 게임 캐릭터가 지나간 길 중 **캐릭터가 처음 걸어본 길의 길이**를 구하려고 합니다. 예를 들어 위의 예시에서 게임 캐릭터가 움직인 길이는 9이지만, 캐릭터가 처음 걸어본 길의 길이는 7이 됩니다. (8, 9번 명령어에서 움직인 길은 2, 3번 명령어에서 이미 거쳐 간 길입니다)

단, 좌표평면의 경계를 넘어가는 명령어는 무시합니다.

예를 들어, LULLLLLLU로 명령했다면

![image](https://res.cloudinary.com/jistring93/image/upload/v1495545063/%EB%B0%A9%EB%AC%B8%EA%B8%B8%EC%9D%B45_nitjwj.png)

- 1번 명령어부터 6번 명령어대로 움직인 후, 7, 8번 명령어는 무시합니다. 다시 9번 명령어대로 움직입니다.

![image](https://res.cloudinary.com/jistring93/image/upload/v1495544946/%EB%B0%A9%EB%AC%B8%EA%B8%B8%EC%9D%B46_nzhumd.png)

이때 캐릭터가 처음 걸어본 길의 길이는 7이 됩니다.

명령어가 매개변수 dirs로 주어질 때, 게임 캐릭터가 처음 걸어본 길의 길이를 구하여 return 하는 solution 함수를 완성해 주세요.

##### 제한사항

- dirs는 string형으로 주어지며, 'U', 'D', 'R', 'L' 이외에 문자는 주어지지 않습니다.
- dirs의 길이는 500 이하의 자연수입니다.

##### 입출력 예

| dirs      | answer |
| --------- | ------ |
| ULURRDLLU | 7      |
| LULLLLLLU | 7      |

##### 입출력 예 설명

입출력 예 #1
 문제의 예시와 같습니다.

입출력 예 #2
 문제의 예시와 같습니다.



출처 : https://programmers.co.kr/learn/courses/30/lessons/49994



---

#### My first solution (Java)

```java
class Solution {
    public int solution(String dirs) {
        int answer = 0;
        int[][] linkCheck = new int[11 * 11][11 * 11];

        int count = 0;
        int position = 60; // 시작은 60번 node
        while (count < dirs.length()) {
            switch (dirs.charAt(count)) {
                case 'U':
                    if ((position - 11) > -1) {
                        if (!visitCheck(linkCheck, position, position - 11)) {
                            linkCheck[position][position - 11] = 1;
                            linkCheck[position - 11][position] = 1;
                            answer++;
                        }
                        position -= 11;
                    }
                    break;
                case 'D':
                    if ((position + 11) < 121) {
                        if (!visitCheck(linkCheck, position, position + 11)) {
                            linkCheck[position][position + 11] = 1;
                            linkCheck[position + 11][position] = 1;
                            answer++;
                        }
                        position += 11;
                    }
                    break;
                case 'L':
                    if (position % 11 != 0) {
                        if (!visitCheck(linkCheck, position, position - 1)) {
                            linkCheck[position][position - 1] = 1;
                            linkCheck[position - 1][position] = 1;
                            answer++;
                        }
                        position -= 1;
                    }
                    break;
                case 'R':
                    if ((position + 1) % 11 != 0) {
                        if (!visitCheck(linkCheck, position, position + 1)) {
                            linkCheck[position][position + 1] = 1;
                            linkCheck[position + 1][position] = 1;
                            answer++;
                        }
                        position += 1;
                    }
                    break;
            }
            count++;
        }
        System.out.println("answer : "+answer);
        return answer;
    }

    public boolean visitCheck(int[][] linkCheck, int x, int y) {
        if (linkCheck[x][y] == 1 || linkCheck[y][x] == 1) {
            return true;
        }
        return false;
    }
}
```

---

#### My logic & feedback

길찾기 문제 중 쉬운 난이도의 문제.

관건은 '방문했던 노드'가 아닌 '방문했던 링크'를 어떻게 Visit check 할 것인가였다.

처음에는 노드 클래스를 만들고, 노드 2개를 가진 링크 클래스를 만들고, 링크 방문체크를 하고...이런 복잡한 방식으로 하려고 하다가, 문제를 풀면서 문득 더 쉬운 방법이 있을 것이라는 확신이 들어서 곰곰히 생각했다.

그러다 떠올린 것이 바로 int 2차원 배열.

Int [ ] [ ] 2차원 배열을 만들면 두 노드 사이의 거리를 표현할 수 있다.

우선 문제에서 주어진 좌표평면은 11x11의 크기이므로 총 121개의 노드를 가지고 있다.

모든 노드에 0~120까지 id를 부여한다고 가정해보자.

그럼 1번노드와 2번노드의 연결은 

int [ 1 ] [ 2 ] = 1  / int [ 2 ] [ 1 ] = 1 (1은 연결되었다는 뜻)

과 같이 표현할 수 있다.

노드의 id는 유일하므로, 위 2차원 배열에 겹치는 link는 없을 것이다.

따라서 복잡하게 클래스, 객체, 해쉬맵 등을 쓰지 않고 간단히 2차원배열로 해당 link를 방문했는지를 체크할 수 있다.



알고리즘 스터디를 시작하고나서부터 이러한 좌표평면 상 그래프 이동 문제를 많이 풀어왔었는데,

여태까지는 방문체크를 전부 클래스 내 멤버변수로 하거나, HashMap을 사용하거나 해왔다.

물론 문제를 푸는데 큰 지장은 없었으나, 가독성이 떨어지고 내부 연산이 늘어나 효과적이지 못한 코드가 된다는 단점이 있었다.

그런 의미에서 본 문제를 풀며 깨달은 2차원 배열의 사용법은 굉장한 Ephipany였다.

예전 스터디 문제중에 못푼 문제가 있는데, 바로 다음 문제다.



https://programmers.co.kr/learn/courses/30/lessons/60063

 

위 문제를 끝내 풀지못했던 이유는 해당 드론이 이동하는 Link의 방문체크를 어떻게 해야할 지 난감해서였는데, 

노드 객체를 만들어 노드로 2칸씩 방문체크를 하다보니 제대로 체크해내기가 너무 버거웠다.

그런데 이제 왠지 2차원 배열로 해결할 수 있을 것 같은 느낌이 든다.

그래서 이번주에 한번 더 도전해볼 생각이다.





