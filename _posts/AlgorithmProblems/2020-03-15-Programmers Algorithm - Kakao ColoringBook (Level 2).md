---
layout: post
title: 프로그래머스 카카오프렌즈 컬러링북 - 2017 카카오코드 (Level 2)
subtitle: Algorithm Solution
background: '/img/bg_technology.jpg'
categories: technology-algorithm

---

##### 문제 설명
카카오 프렌즈 컬러링북
출판사의 편집자인 어피치는 네오에게 컬러링북에 들어갈 원화를 그려달라고 부탁하여 여러 장의 그림을 받았다. 
여러 장의 그림을 난이도 순으로 컬러링북에 넣고 싶었던 어피치는 영역이 많으면 색칠하기가 까다로워 어려워진다는 사실을 발견하고 
그림의 난이도를 영역의 수로 정의하였다. (영역이란 상하좌우로 연결된 같은 색상의 공간을 의미한다.)

그림에 몇 개의 영역이 있는지와 가장 큰 영역의 넓이는 얼마인지 계산하는 프로그램을 작성해보자.


##### 입력 형식
입력은 그림의 크기를 나타내는 m과 n, 그리고 그림을 나타내는 m × n 크기의 2차원 배열 picture로 주어진다. 제한조건은 아래와 같다.

1 <= m, n <= 100
picture의 원소는 0 이상 2^31 - 1 이하의 임의의 값이다.
picture의 원소 중 값이 0인 경우는 색칠하지 않는 영역을 뜻한다.
출력 형식
리턴 타입은 원소가 두 개인 정수 배열이다. 그림에 몇 개의 영역이 있는지와 가장 큰 영역은 몇 칸으로 이루어져 있는지를 리턴한다.

##### 예제 입출력
m	n	picture	answer
6	4	[[1, 1, 1, 0], [1, 2, 2, 0], [1, 0, 0, 1], [0, 0, 0, 1], [0, 0, 0, 3], [0, 0, 0, 3]]	[4, 5]
예제에 대한 설명
예제로 주어진 그림은 총 4개의 영역으로 구성되어 있으며, 왼쪽 위의 영역과 오른쪽의 영역은 모두 1로 구성되어 있지만 
상하좌우로 이어져있지 않으므로 다른 영역이다. 가장 넓은 영역은 왼쪽 위 1이 차지하는 영역으로 총 5칸이다.


출처 : https://programmers.co.kr/learn/courses/30/lessons/1829

---



#### My solution (Java)

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.LinkedList;
import java.util.Queue;
class Solution {
	ArrayList<Pixel> pix = new ArrayList<Pixel>();
	HashMap<Integer, Boolean> visitCheck = new HashMap<Integer, Boolean>();
	int totalBfs = 0;

	public int[] solution(int m, int n, int[][] picture) {
		int numberOfArea = 0;
		int maxSizeOfOneArea = 0;
		int id = 0;
		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
				ArrayList<Pixel> linked = new ArrayList<Pixel>();
				Pixel pixel = new Pixel(id, picture[i][j], linked, i, j);
				pix.add(pixel);
				visitCheck.put(pixel.id, false);
				id++;
				System.out.print(pixel.number + " ");
			}
			System.out.println();
		}

		/* linked 만들기 (0제외) */
		for (int i = 0; i < pix.size(); i++) {
			Pixel cel = pix.get(i);
			int number = cel.number;
			if (number == 0) {
				continue;
			}
			int x = cel.x;
			int y = cel.y;
			/* 오른쪽 */
			if (y != n - 1) {
				if (pix.get(i + 1).number == number) {
					cel.linked.add(pix.get(i + 1));
				}
			}
			/* 왼쪽 */
			if (y != 0) {
				if (pix.get(i - 1).number == number) {
					cel.linked.add(pix.get(i - 1));
				}
			}
			/* 위쪽 */
			if (x != 0) {
				if (pix.get(i - n).number == number) {
					cel.linked.add(pix.get(i - n));
				}
			}
			/* 아래쪽 */
			if (x != m - 1) {
				if (pix.get(i + n).number == number) {
					cel.linked.add(pix.get(i + n));
				}
			}
		}

		/* 0을 제외한 픽셀 전체 for문으로 돌면서 BFS 탐색 */
		int huggestArea = 0;
		for (int i = 0; i < pix.size(); i++) {
			if (pix.get(i).number == 0) {
				continue;
			}
			if (visitCheck.get(pix.get(i).id)) {
				continue;
			}
			huggestArea = Math.max(huggestArea, bfs(pix.get(i)));
		}

		int[] answer = new int[2];
		numberOfArea = totalBfs;
		answer[0] = numberOfArea;
		maxSizeOfOneArea = huggestArea;
		answer[1] = maxSizeOfOneArea;
		return answer;
	}

	public int bfs(Pixel pix) {
		totalBfs++;
		Queue<Pixel> que = new LinkedList<Pixel>();
		que.add(pix);
		int count = 0;
		visitCheck.replace(pix.id, true);

		while (true) {
			if (que.isEmpty()) {
				break;
			}
			Pixel pixOut = que.poll();
			count++;
			for (int i = 0; i < pixOut.linked.size(); i++) {
				if (visitCheck.get(pixOut.linked.get(i).id)) {
					continue;
				}
				que.add(pixOut.linked.get(i));
				visitCheck.replace(pixOut.linked.get(i).id, true);
			}
		}
		return count;
	}
}

class Pixel {
	int id;
	int number;
	int x;
	int y;
	ArrayList<Pixel> linked = new ArrayList<Pixel>();

	public Pixel(int id, int number, ArrayList<Pixel> linked, int x, int y) {
		this.id = id;
		this.number = number;
		this.linked = linked;
		this.x = x;
		this.y = y;
	}
}
```



---



#### Feedback

```
그래프 문제지만 어렵지 않다. 그래프 탐색의 가장 기초 유형이라 할 수 있을 것 같다.

나의 logic
 1. 주어진 2차원 배열로 리스트를 만든다(ArrayList) 
 2. 리스트는 Pixel 객체들로 구성되며, Pixel 본인의 상하좌우를 탐색하여 number가 같은 Pixel이 있으면 
 Pixel의 멤버변수인 linked(ArrayList)에 담는다. 단, 0은 제외한다. 
 3. 픽셀맵의 id를 기준으로 for문으로 도는데, 픽셀마다 BFS를 돈다. 단, 0은 제외한다. 
 4. 해당 pixel이 갖고있는 linked의 number가 해당 픽셀의 number와 일치하면 방문체크한다. 
 5.BFS를 돈 횟수(전체가 몇 개의 영역인지), BFS를 한 번 돌 때 몇 개의 Pixel을 방문하는지(각 영역의 크기는 얼마인지)를 반환한다.
 6. for문이 끝나면 6번의 두 값을 반환한다.
```

