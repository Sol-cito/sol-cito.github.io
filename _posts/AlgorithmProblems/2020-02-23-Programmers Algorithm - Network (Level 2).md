---
layout: post
title: 프로그래머스 BFS/DFS -  네트워크 (Level 3)
subtitle: Programmers Algorithm Test Solution
background: '/img/bg_technology.jpg'
categories: technology/tech-algorithm
---


##### 문제 설명 
네트워크란 컴퓨터 상호 간에 정보를 교환할 수 있도록 연결된 형태를 의미합니다. 예를 들어, 컴퓨터 A와 컴퓨터 B가 직접적으로 연결되어있고, 컴퓨터 B와 컴퓨터 C가 직접적으로 연결되어 있을 때 컴퓨터 A와 컴퓨터 C도 간접적으로 연결되어 정보를 교환할 수 있습니다. 따라서 컴퓨터 A, B, C는 모두 같은 네트워크 상에 있다고 할 수 있습니다.

컴퓨터의 개수 n, 연결에 대한 정보가 담긴 2차원 배열 computers가 매개변수로 주어질 때, 네트워크의 개수를 return 하도록 solution 함수를 작성하시오.

##### 제한사항
컴퓨터의 개수 n은 1 이상 200 이하인 자연수입니다.
각 컴퓨터는 0부터 n-1인 정수로 표현합니다.
i번 컴퓨터와 j번 컴퓨터가 연결되어 있으면 computers[i][j]를 1로 표현합니다.
computer[i][i]는 항상 1입니다.

##### 입출력 예
n	computers	return
3	[[1, 1, 0], [1, 1, 0], [0, 0, 1]]	2
3	[[1, 1, 0], [1, 1, 1], [0, 1, 1]]	1

출처 : https://programmers.co.kr/learn/courses/30/lessons/43162

---

#### My solution (Java)

```java
import java.util.ArrayList;
import java.util.HashMap;

class Solution {
	HashMap<Integer, Boolean> visit = new HashMap<Integer, Boolean>();

	public int solution(int n, int[][] computers) {
		int answer = 0;
		HashMap<Integer, ArrayList<Integer>> nodeList = new HashMap<Integer, ArrayList<Integer>>();
		/* 각 노드를 HashMap에 담음과 동시에, */
		/* Value의 List에 각 노드와 연결된 다른 노드의 Number를 담는다 */
		for (int i = 1; i <= n; i++) {
			ArrayList<Integer> linked = new ArrayList<Integer>();
			nodeList.put(i, linked);
			visit.put(i, false);
			for (int j = 1; j <= n; j++) {
				if (j == i) {
					continue;
				}
				if (computers[i - 1][j - 1] == 1) {
					nodeList.get(i).add(j);
				}
			}
		}

		for (int i = 1; i <= nodeList.size(); i++) {
			if (visit.get(i) == true) {
				continue;
			}
			visit.replace(i, true);
			recursion(nodeList, i);
			answer++;
		}
		System.out.println(answer);
		return answer;
	}
	
    /* 재귀 함수. Node에 연결된 다른 하위 Node가 없으면 return한다 */
	public void recursion(HashMap<Integer, ArrayList<Integer>> nodeList, int start) {
		if (nodeList.get(start).size() == 0) {
			return;
		}

		for (int i = 0; i < nodeList.get(start).size(); i++) {
			if (visit.get(nodeList.get(start).get(i)) == true) {
				continue;
			}
			if (visit.get(nodeList.get(start).get(i)) == false) {
				visit.replace(nodeList.get(start).get(i), true);
				recursion(nodeList, nodeList.get(start).get(i));
			}
		}

	}
}
```

---

#### Feedback

```
이 문제는 사실 3주 전 알고리즘 스터디에서, 내가 끝끝내 풀지 못한 문제다.
그런데 이번 주 '가장 긴 노드' 문제(이것도 결국 해결하지 못했음. 이번 주말에 재도전 예정이다)를 풀기위해 끙끙대고, 동기들한테서 풀이 설명을 듣고나서 다시 재도전을 했는데, 20분만에 답이 나왔다.
3주만에 실력이 그래도 조금 오른건지, 너무 기분이 좋았다.

나의 Logic
1. 노드를 HashMap에 담는데, 각 노드의 Value는 그 노드와 연결된(자기자신 제외) 다른 노드의 번호이다. 즉 HashMap<Integer, ArrayList<Integer>> 이다.
2. int[][] computer 를 for문으로 돌면서 각 노드의 Value(ArrayList)에 연결 노드의 번호를 담는다.
3. 그리고 '방문하지 않은' 노드를 for문으로 탐색하며 재귀 함수를 호출한다.
4. 재귀함수는 우선 노드를 방문체크한 후, Value에 연결된 다른 노드들에 재귀를 돌아 DFS를 구현한다. 재귀함수는 더 이상 깊이 내려갈 곳이 없을 때(연결된 다른 노드가 없을 때) return된다.
5. 3번의 for문이 한 번 돌 때마다 answer++ 를 한다(이미 방문해서 continue되는 노드 제외)
```

