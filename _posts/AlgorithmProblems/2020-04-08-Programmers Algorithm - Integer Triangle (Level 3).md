---
layout: post
title: "프로그래머스 다이나믹 프로그래밍 - 정수 삼각형 (Level 3)"
subtitle: Algorithm Solution
background: '/img/bg_technology.jpg'
categories: technology
---

###### 문제 설명

![스크린샷 2018-09-14 오후 5.44.19.png](https://grepp-programmers.s3.amazonaws.com/files/production/97ec02cc39/296a0863-a418-431d-9e8c-e57f7a9722ac.png)

위와 같은 삼각형의 꼭대기에서 바닥까지 이어지는 경로 중, 거쳐간 숫자의 합이 가장 큰 경우를 찾아보려고 합니다. 아래 칸으로 이동할 때는 대각선 방향으로 한 칸 오른쪽 또는 왼쪽으로만 이동 가능합니다. 예를 들어 3에서는 그 아래칸의 8 또는 1로만 이동이 가능합니다.

삼각형의 정보가 담긴 배열 triangle이 매개변수로 주어질 때, 거쳐간 숫자의 최댓값을 return 하도록 solution 함수를 완성하세요.

##### 제한사항

- 삼각형의 높이는 1 이상 500 이하입니다.
- 삼각형을 이루고 있는 숫자는 0 이상 9,999 이하의 정수입니다.

##### 입출력 예

| triangle                                                | result |
| ------------------------------------------------------- | ------ |
| [[7], [3, 8], [8, 1, 0], [2, 7, 4, 4], [4, 5, 2, 6, 5]] | 30     |

출처 : https://programmers.co.kr/learn/courses/30/lessons/43105



---

#### My first solution (Java)

```java
class Solution {
	public int solution(int[][] triangle) {
		int answer = 0;

		Node[][] nodeList = new Node[triangle.length][triangle.length];

		/* Node 객체 생성 */
		int length_j = 1;
		for (int i = 0; i < triangle.length; i++) {
			for (int j = 0; j < length_j; j++) {
				Node node = new Node(triangle[i][j], 0);
				nodeList[i][j] = node;
			}
			length_j++;
		}
		
		/* triangle 배열을 따라 한 줄씩 내려가면서 total을 더하는데, 내려오는 것들 중 큰 것만 선택해서 node에 담기 */
		length_j = 1;
		for (int i = 0; i < triangle.length - 1; i++) {
			for (int j = 0; j < length_j; j++) {
				Node node = nodeList[i][j];
				int upperNodeTotal = node.total;
				int upperNodeNumber = node.number;

				Node leftNode = nodeList[i + 1][j];
				if (leftNode.total < upperNodeTotal + upperNodeNumber) {
					nodeList[i + 1][j].total = upperNodeTotal + upperNodeNumber;
				}
				Node rightNode = nodeList[i + 1][j + 1];
				if (rightNode.total < upperNodeTotal + upperNodeNumber) {
					nodeList[i + 1][j+1].total = upperNodeTotal + upperNodeNumber;
				}
			}
			length_j++;
		}
		
		for(int i = 0; i < triangle.length; i++) {
			Node node = nodeList[triangle.length - 1][i];
			int total = node.total;
			int number = node.number;
			answer = Math.max(total+number, answer);
		}
		return answer;
	}
}

class Node {
	int number;
	int total;

	public Node(int number, int total) {
		this.number = number;
		this.total = total;
	}
}

```

---

#### My logic & feedback

어려운 문제는 아니나, 풀이 방법을 생각해내는 데 생각보다 시간이 오래 걸렸다.

처음 풀이를 단순히 재귀를 통해 풀려고 했는데 시간초과가 났다.

다시 생각해보니 재귀로 모든 경우의 수를 다 따지면 시간복잡도가 O(2^n) 이 나오는 무시무시한 사태가...

그래서 다시 생각을 가다듬고 다른 방법으로 접근했다.

문제의 핵심은, 아래 줄에 위치하는 각각의 점들이, 위에서 내려오는 '가장 큰 값'만 받아서 계산하면 된다는 점이다. 모든 값을 다 받아서 계산할 필요가 없다.

그래서 나는 각 점을 Node로 클래스화 한 뒤, 클래스에 total이라는 변수를 통해 가장 큰 값만을 total에 저장하여 맨 마지막 줄에 있는 놈들 중 total이 가장 큰 놈을 답으로 하는 방법으로 풀었다.

그런데..

다 풀고나서 다른 사람의 풀이를 보니, 굳이 내 방법처럼 Node 클래스를 따로 만들지 않고 Triangle의 점 하나 하나를 그때 그때 바꾸어가며(그래서 Dynamic programming인가보다) 푸는 것이 훨씬 효율적이라는 것을 깨달았다.

굳이 점 하나하나를 Node로 생각하지 말고, 한 점의 수 자체를 그 때 그 때 바꾸면 되는 것이다!

예를 들어 

​    	**7**

  	**3  8**

​	**8  1  0**

 **2   4   4   4** 

이렇게 삼각형이 있으면, 위에서 내려오는 값을 통해 두 번째 라인의 값을 (3+7), (8+7)로 바꾸고,

​    	**7**

​	**10  15**

​	**8  1  0**

 **2   4   4   4** 

또 세 번째 라인을 바꾸되 큰 값을 기준으로 바꾸어 나가면 되는 것이다. (8+10) / (1+15) / (0+15)

​      	**7**

​	  **10  15**

   **18  16  15**

 **2    4    4   4** 

이렇게 한 경우가 지나갈 때 마다 즉각즉각 값을 바꾸면서 최적의 결과만을 본다면, 완전탐색보다 훨씬 성능좋은 결과를 낼 수 있을 것이다.