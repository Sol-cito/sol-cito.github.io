---
layout: post
title: 프로그래머스 그래프 - 가장 먼 노드 (Level 3)
subtitle: Programmers Algorithm Test Solution
background: '/img/bg_technology.jpg'
categories: technology
---


```
문제 설명
n개의 노드가 있는 그래프가 있습니다. 각 노드는 1부터 n까지 번호가 적혀있습니다. 1번 노드에서 가장 멀리 떨어진 노드의 갯수를 구하려고 합니다. 가장 멀리 떨어진 노드란 최단경로로 이동했을 때 간선의 개수가 가장 많은 노드들을 의미합니다.

노드의 개수 n, 간선에 대한 정보가 담긴 2차원 배열 vertex가 매개변수로 주어질 때, 1번 노드로부터 가장 멀리 떨어진 노드가 몇 개인지를 return 하도록 solution 함수를 작성해주세요.

제한사항
노드의 개수 n은 2 이상 20,000 이하입니다.
간선은 양방향이며 총 1개 이상 50,000개 이하의 간선이 있습니다.
vertex 배열 각 행 [a, b]는 a번 노드와 b번 노드 사이에 간선이 있다는 의미입니다.
입출력 예
n	vertex	return
6	[[3, 6], [4, 3], [3, 2], [1, 3], [1, 2], [2, 4], [5, 2]]	3
입출력 예 설명
예제의 그래프를 표현하면 아래 그림과 같고, 1번 노드에서 가장 멀리 떨어진 노드는 4,5,6번 노드입니다.

출처 : https://programmers.co.kr/learn/courses/30/lessons/49189
```

---

#### My solution (Java)

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.LinkedList;
import java.util.Queue;

class Solution {
	HashMap<Integer, Boolean> visit = new HashMap<Integer, Boolean>();

	public int solution(int n, int[][] edge) {
		int answer = 0;
		HashMap<Integer, Node> nodeList = new HashMap<Integer, Node>();
		for (int i = 1; i <= n; i++) {
			ArrayList<Integer> linked = new ArrayList<Integer>();
			Node node = new Node(i, linked, 0);
			visit.put(i, false);
			nodeList.put(i, node);
		}
		/* edge[][] 의 연결 관계를 탐색하면서 각 Node의 linked를 채워줌 */
		for (int i = 0; i < edge.length; i++) {
			nodeList.get(edge[i][0]).linked.add(edge[i][1]);
		}
		for (int i = 0; i < edge.length; i++) {
			nodeList.get(edge[i][1]).linked.add(edge[i][0]);
		}

		Queue<Node> que = new LinkedList<Node>();
		que.add(nodeList.get(1));
		visit.replace(1, true);
		int depth = 0;
		while (true) {
			if (que.isEmpty()) {
				break;
			}
			Node node = que.peek();
			for (int i = 0; i < node.linked.size(); i++) {
				if (visit.get(node.linked.get(i)) == false) {
					nodeList.get(node.linked.get(i)).changeDepth(node.depth + 1);
					que.add(nodeList.get(node.linked.get(i)));
					visit.replace(node.linked.get(i), true);
				}
			}
			depth = node.depth;
			que.poll();
		}
		for (int i = 1; i <= nodeList.size(); i++) {
			if (nodeList.get(i).depth == depth) {
				answer++;
			}
		}
		System.out.println(answer);
		return answer;
	}
}

class Node {
	int id;
	ArrayList<Integer> linked = new ArrayList<Integer>();
	int depth;

	public Node(int id, ArrayList<Integer> linked, int depth) {
		this.id = id;
		this.linked = linked;
		this.depth = depth;
	}

	public void changeDepth(int num) {
		depth = num;
	}
}
```

---

#### Feedback

```
이 문제는 스터디 당시에는 결국 풀지 못한 문제였다. 원인은 두 가지로,
1) BFS로 풀어야 한다는 점은 캐치했으나, Queue로 BFS를 구현하는 방법을 몰라 대신 HashMap과 ArrayList로 풀려고 시도하였다. 그러나 선형이 아닌, 노드가 모두 연결되어있는 원형 그래프의 경우 이 방법으로 풀기에는 적합하지 않아 실패하였다.
2) 내부 클래스로 Node 클래스를 새로 만드는 방법을 시도하지 않았다.

즉, 그래프 문제 자체에 익숙하지 않았기 때문에 풀이에 실패했다고 할 수 있다.

스터디 이후, Queue로 BFS를 구현하는 방법을 유투브로 추가로 찾아보고 Node 클래스를 어떻게 만들까 고민하면서 다시 문제풀이를 시도했고, 결국 한 방에 성공하였다! 
그래프 탐색은 선형적인 문제가 아니지만 굉장히 흥미로웠으므로, 풀이 방법 및 Logic을 잘 기억해두면 좋을 것 같다.

나의 Logic
1. Node 내부클래스를 만든다. Node의 번호 id, 해당 Node와 연결되어있는 다른 Node의 번호가 담긴 ArrayList, 그리고 depth(1번 노드와의 거리를 나타낸다)를 멤버 변수로 선언한다.
2. Node 내부클래스에 파라미터에 따라 depth가 변하도록 하는 메소드를 만든다(추후 각 노드를 Queue에 담을 때 1번 노드와 얼마나 떨어져있는지-얼마나 깊이 내려가는지를 보여주는 integer).
3. 이제 Solution 클래스에서 방문을 체크할 visit HashMap을 선언하고, n개의 노드 객체를 만들면서(depth는 0으로 설정) 동시에 visit을 false로 만들어 채워준다. 또한 HashMap으로 NodeList를 만들어 만들어진 Node 객체를 담아준다.
4. edge[][] 배열을 for문으로 돌면서 각 Node에 담긴 ArrayList에 연결된 다른 Node 번호를 담아준다.
5. Queue를 생성하고 반복문으로 들어간다.
 1) Queue에 Node를 담고, visit을 true로 만든다.
 2) peek()로 Node 하나를 꺼내서 보고, 그 Node의 ArrayList 변수에 있는 다른 Node들 중 visit이 false인 것들만 다시 Queue에 담아준다(담으면서 visit을 true로 만든다). 담는 노드들은 depth멤버변수가 1 증가하는 chageDepth 메소드를 실행해준다.
 3) poll()로 맨 마지막 노드를 제거한다.
 4) isEmpty == true 면 반복문을 종료한다.
6. 반복문이 종료되고 난 후 마지막으로 Queue에 넣은 Node들의 depth에 해당하는 것들만 nodeList에서 찾아 개수를 헤아려주면, 그게 answer다.
```

