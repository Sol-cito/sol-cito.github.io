---
layout: post
title: 프로그래머스 완전탐색 - 카펫(Level 2)
subtitle: Programmers Algorithm Test Solution
background: '/img/bg_technology.jpg'
categories: technology
---


```
문제 설명
Leo는 카펫을 사러 갔다가 아래 그림과 같이 중앙에는 빨간색으로 칠해져 있고 모서리는 갈색으로 칠해져 있는 격자 모양 카펫을 봤습니다.

Leo는 집으로 돌아와서 아까 본 카펫의 빨간색과 갈색으로 색칠된 격자의 개수는 기억했지만, 전체 카펫의 크기는 기억하지 못했습니다.

Leo가 본 카펫에서 갈색 격자의 수 brown, 빨간색 격자의 수 red가 매개변수로 주어질 때 카펫의 가로, 세로 크기를 순서대로 배열에 담아 return 하도록 solution 함수를 작성해주세요.

제한사항
갈색 격자의 수 brown은 8 이상 5,000 이하인 자연수입니다.
빨간색 격자의 수 red는 1 이상 2,000,000 이하인 자연수입니다.
카펫의 가로 길이는 세로 길이와 같거나, 세로 길이보다 깁니다.
입출력 예
brown	red	return
10	2	[4, 3]
8	1	[3, 3]
24	24	[8, 6]

출처 : https://programmers.co.kr/learn/courses/30/lessons/42842
```

---

#### My solution (Java)

```java
class Solution {
	public int[] solution(int brown, int red) {
		int[] answer = {};
		int sum = brown + red;
        answer = hoXver(sum, red);
		return answer;
	}

	public int[] hoXver(int sum, int red) {
		int ver = 1; // 세로 1
		int hor = sum / ver; // 가로 = sum/세로
		int[] result = new int[2];
		while (true) {
		/* 이 지점에서 red카펫을 탐색 */
		/* 어떻게 탐색 ? 가로-2 x 세로-2 의 공간을, red카펫이 감당할 수 있는지 보면 됨. */
			if ((hor - 2) * (ver - 2) >= red) {
				result[0] = hor;
				result[1] = ver;
				return result;
			}
			/* 나머지 0 일 때 세로를 1 증가시키고 가로는 그만큼 감소 */
			while (true) {
				ver++;
				if (sum % ver == 0) {
					hor = sum / ver;
					break;
				}
			}
			if (ver > hor) {
				int x = ver;
				ver = hor;
				hor = x;
				break;
			}
		}
		return null;
	}
}
```

---

#### Feedback

```
나의 Logic

1. 갈색 격자와 빨간 격자를 합(sum)한 수는 카펫의 넓이 이므로, 두 자연수의 곱으로 가로x세로 를 표현할 수 있다.
 Ex) 갈색이 10, 빨강이 2라면 넓이는 12이고, [가로 >= 세로] 이므로 12x1 / 6x2 / 4x3 의 세 가지 경우로 표현될 수 있다.
2. 이처럼 갈색과 빨강의 합에서 가로x세로의 경우의 수를 모두 탐색하고(세로가 1에서 +1씩 증가하며 가로, 세로가 자연수인 경우를 탐색), 세로의 길이가 가로 길이보다 커지는 순간 break한다.
3. 빨강 격자가 갈색 카펫 가운데에 있으려면 적어도 전체 카펫의 가로길이, 세로길이 각각에 -2를 해서 곱한 넓이공간보다 같거나 작아야 하므로 (카펫의 전체 모양을 생각해보자), 2에서 탐색한 각각의 경우의 수가 "(hor - 2) * (ver - 2) >= red" 조건을 만족하는지 check한다.
4. 만족하면 그 결과값을 return한다.

생각보다 쉽게 답이 나왔는데, 프로그래머스에서 다른사람의 풀이를 보니 이건 뭐...코드 한줄로 푼 사람도 있고, 여튼 쉬운 문제였던 것 같다. 쉽게 풀어낸 사람들의 코드를 해석해보면 좋을 것 같다.
```

