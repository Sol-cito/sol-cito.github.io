---
layout: post
title: "프로그래머스 그래프 - 순위 (Level 3)"
subtitle: Algorithm Solution
background: '/img/bg_technology.jpg'
categories: technology-algorithm

---

###### 문제 설명

n명의 권투선수가 권투 대회에 참여했고 각각 1번부터 n번까지 번호를 받았습니다. 권투 경기는 1대1 방식으로 진행이 되고, 만약 A 선수가 B 선수보다 실력이 좋다면 A 선수는 B 선수를 항상 이깁니다. 심판은 주어진 경기 결과를 가지고 선수들의 순위를 매기려 합니다. 하지만 몇몇 경기 결과를 분실하여 정확하게 순위를 매길 수 없습니다.

선수의 수 n, 경기 결과를 담은 2차원 배열 results가 매개변수로 주어질 때 정확하게 순위를 매길 수 있는 선수의 수를 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 선수의 수는 1명 이상 100명 이하입니다.
- 경기 결과는 1개 이상 4,500개 이하입니다.
- results 배열 각 행 [A, B]는 A 선수가 B 선수를 이겼다는 의미입니다.
- 모든 경기 결과에는 모순이 없습니다.

##### 입출력 예

| n    | results                                  | return |
| ---- | ---------------------------------------- | ------ |
| 5    | [[4, 3], [4, 2], [3, 2], [1, 2], [2, 5]] | 2      |

##### 입출력 예 설명

2번 선수는 [1, 3, 4] 선수에게 패배했고 5번 선수에게 승리했기 때문에 4위입니다.
5번 선수는 4위인 2번 선수에게 패배했기 때문에 5위입니다.

출처 :https://programmers.co.kr/learn/courses/30/lessons/49191



---

#### My first solution (Java)

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.LinkedList;
import java.util.Queue;

class Solution {
	public int solution(int n, int[][] results) {
		int answer = 0;

		HashMap<Integer, Athelete> map = new HashMap<Integer, Athelete>();
		/* 선수 객체 생성 */
		for (int i = 1; i <= n; i++) {
			ArrayList<Integer> left = new ArrayList<Integer>();
			ArrayList<Integer> right = new ArrayList<Integer>();
			Athelete athelete = new Athelete(i, left, right);
			map.put(i, athelete);
		}

		/* 운동선수 객체의 left, right 담아주기 */
		for (int i = 0; i < results.length; i++) {
			int winner = results[i][0];
			map.get(results[i][1]).left.add(winner);
			int loser = results[i][1];
			map.get(results[i][0]).right.add(loser);
		}

		/* BFS 탐색으로 왼쪽, 오른쪽 수 헤아리기 */
		for (int i = 1; i <= n; i++) {
			if (BFS(i, map) == n - 1) {
				answer++;
			}
		}
		return answer;
	}

	public int BFS(int id, HashMap<Integer, Athelete> map) {
		Athelete athelete = map.get(id);

		Queue<Athelete> leftQueue = new LinkedList<Athelete>();
		leftQueue.add(athelete);
		int total = 0;
		int[] leftVisitCheck = new int[map.size()];
		leftVisitCheck[id - 1] = 1;
        
		while (true) {
			if (leftQueue.isEmpty()) {
				break;
			}
			Athelete targetAthelete = leftQueue.poll();
			ArrayList<Integer> left = targetAthelete.left;

			for (int i = 0; i < left.size(); i++) {
				if (leftVisitCheck[left.get(i) - 1] == 1) {
					continue;
				}
				leftQueue.add(map.get(left.get(i)));
				leftVisitCheck[left.get(i) - 1] = 1;
				total++;
			}
		}

		Queue<Athelete> rightQueue = new LinkedList<Athelete>();
		rightQueue.add(athelete);
		int[] rightVisitCheck = new int[map.size()];
		rightVisitCheck[id - 1] = 1;
        
		while (true) {
			if (rightQueue.isEmpty()) {
				break;
			}
			Athelete targetAthelete = rightQueue.poll();
			ArrayList<Integer> right = targetAthelete.right;

			for (int i = 0; i < right.size(); i++) {
				if (rightVisitCheck[right.get(i) - 1] == 1) {
					continue;
				}
				rightQueue.add(map.get(right.get(i)));
				rightVisitCheck[right.get(i) - 1] = 1;

				total++;
			}
		}
		return total;
	}

}

class Athelete {
	int id;
	ArrayList<Integer> left;
	ArrayList<Integer> right;

	public Athelete(int id, ArrayList<Integer> left, ArrayList<Integer> right) {
		this.id = id;
		this.left = left;
		this.right = right;
	}
}
```

---

#### My logic & feedback

쉬운 문제. 

'자신과 연결되어있는 양 옆의 모든 선수들의 숫자의 합이 n-1 이면 그 선수의 순위를 알 수 있다 ' 는 로직을 바탕으로 풀었다.

선수 클래스를 만들고, 객체를 생성한 후 results 배열을 통해 네트워크를 만들어 준다.

선수 클래스는 left, right 리스트가 있는데, 각각 자신보다 순위가 높은 선수의 id(Left), 순위가 낮은 선수의 id(Right)로 이루어져 있으며,

선수 클래스를 만든 후 left, right로 각각 Queue를 이용한 BFS를 돌면서 자신의 양 옆에 몇 명의 선수가 있는지 검사하면 된다.