---
layout: post
title: 프로그래머스 배달 - 2018 Summer/Winter Coding (Level 3)
subtitle: Algorithm Solution
background: '/img/bg_technology.jpg'
categories: technology-algorithm

---

###### **문제 설명**

N개의 마을로 이루어진 나라가 있습니다. 이 나라의 각 마을에는 1부터 N까지의 번호가 각각 하나씩 부여되어 있습니다. 각 마을은 양방향으로 통행할 수 있는 도로로 연결되어 있는데, 서로 다른 마을 간에 이동할 때는 이 도로를 지나야 합니다. 도로를 지날 때 걸리는 시간은 도로별로 다릅니다. 현재 1번 마을에 있는 음식점에서 각 마을로 음식 배달을 하려고 합니다. 각 마을로부터 음식 주문을 받으려고 하는데, N개의 마을 중에서 K 시간 이하로 배달이 가능한 마을에서만 주문을 받으려고 합니다. 다음은 N = 5, K = 3인 경우의 예시입니다.



위 그림에서 1번 마을에 있는 음식점은 [1, 2, 4, 5] 번 마을까지는 3 이하의 시간에 배달할 수 있습니다. 그러나 3번 마을까지는 3시간 이내로 배달할 수 있는 경로가 없으므로 3번 마을에서는 주문을 받지 않습니다. 따라서 1번 마을에 있는 음식점이 배달 주문을 받을 수 있는 마을은 4개가 됩니다.
마을의 개수 N, 각 마을을 연결하는 도로의 정보 road, 음식 배달이 가능한 시간 K가 매개변수로 주어질 때, 음식 주문을 받을 수 있는 마을의 개수를 return 하도록 solution 함수를 완성해주세요.

##### 제한사항

- 마을의 개수 N은 1 이상 50 이하의 자연수입니다.

- road의 길이(도로 정보의 개수)는 1 이상 2,000 이하입니다.

- road의 각 원소는 마을을 연결하고 있는 각 도로의 정보를 나타냅니다.

- road는 길이가 3인 배열이며, 순서대로 (a, b, c)를 나타냅니다.

  - a, b(1 ≤ a, b ≤ N, a != b)는 도로가 연결하는 두 마을의 번호이며, c(1 ≤ c ≤ 10,000, c는 자연수)는 도로를 지나는데 걸리는 시간입니다.
  - 두 마을 a, b를 연결하는 도로는 여러 개가 있을 수 있습니다.
  - 한 도로의 정보가 여러 번 중복해서 주어지지 않습니다.

- K는 음식 배달이 가능한 시간을 나타내며, 1 이상 500,000 이하입니다.

- 임의의 두 마을간에 항상 이동 가능한 경로가 존재합니다.

- 1번 마을에 있는 음식점이 K 이하의 시간에 배달이 가능한 마을의 개수를 return 하면 됩니다.

  ##### 입출력 예

  | N    | road                                                      | K    | result |
  | ---- | --------------------------------------------------------- | ---- | ------ |
  | 5    | [[1,2,1],[2,3,3],[5,2,2],[1,4,2],[5,3,1],[5,4,2]]         | 3    | 4      |
  | 6    | [[1,2,1],[1,3,2],[2,3,2],[3,4,3],[3,5,2],[3,5,3],[5,6,1]] | 4    | 4      |

  ##### 입출력 예 설명

  입출력 예 #1
  문제의 예시와 같습니다.

  입출력 예 #2
  주어진 마을과 도로의 모양은 아래 그림과 같습니다.

  1번 마을에서 배달에 4시간 이하가 걸리는 마을은 [1, 2, 3, 5] 4개이므로 4를 return 합니다.



출처 : https://programmers.co.kr/learn/courses/30/lessons/12978



---

#### My first solution (Java) - Fail (exceeds time limit)

```java
import java.util.ArrayList;
import java.util.HashMap;

class Solution {
	HashMap<Integer, Boolean> finalResultMap = new HashMap<Integer, Boolean>();
	HashMap<Integer, Village> villageMap = new HashMap<Integer, Village>();

	public int solution(int N, int[][] road, int K) {
		int answer = 0;
		HashMap<Integer, Boolean> visitCheck = new HashMap<Integer, Boolean>();
		/* 마을 객체 생성 */
		for (int i = 1; i <= N; i++) {
			ArrayList<Road> roadList = new ArrayList<Road>();
			Village village = new Village(i, roadList);
			visitCheck.put(i, false);
			villageMap.put(i, village);
			finalResultMap.put(i, false);
		}

		/* 도로 객체 생성하고 village의 list에 add */
		for (int i = 0; i < road.length; i++) {
			int village1 = road[i][0];
			int village2 = road[i][1];
			int time = road[i][2];
			Road roadObject = new Road(village1, village2, time);
			villageMap.get(village1).roadList.add(roadObject);
			villageMap.get(village2).roadList.add(roadObject);
		}
		answer++;
		recursion(1, visitCheck, K, 0);
		for (int i = 1; i <= finalResultMap.size(); i++) {
			if (finalResultMap.get(i) == true) {
				answer++;
			}
		}
		return answer;
	}

	/* 배달 recursion */
	public void recursion(int villageId, HashMap<Integer, Boolean> visitCheck, int K, int totalTime) {
		ArrayList<Road> roadBelongs = villageMap.get(villageId).roadList;
		HashMap<Integer, Boolean> newVistiCheck = new HashMap<Integer, Boolean>();
		for (int x = 1; x <= visitCheck.size(); x++) {
			if (visitCheck.get(x) == true) {
				newVistiCheck.put(x, true);
			} else {
				newVistiCheck.put(x, false);
			}
		}
		newVistiCheck.replace(villageId, true);
		for (int i = 0; i < roadBelongs.size(); i++) {
			int newTotalTime = totalTime;
            // 재귀를 도는 villageId와 연결된 village의 ID
			int linkedId = roadBelongs.get(i).linkedVilage(villageId); 
          	if (newVistiCheck.get(linkedId) == true) {
				continue;
			}
			int time = roadBelongs.get(i).time;
			newTotalTime += time;
			if (newTotalTime <= K) {
				finalResultMap.replace(linkedId, true);
				recursion(linkedId, newVistiCheck, K, newTotalTime);
			}
		}
	}
}

class Village {
	int id;
	ArrayList<Road> roadList = new ArrayList<Road>();

	public Village(int id, ArrayList<Road> roadList) {
		this.id = id;
		this.roadList = roadList;
	}
}

class Road {
	int village1;
	int village2;
	int time;

	public Road(int village1, int village2, int time) {
		this.village1 = village1;
		this.village2 = village2;
		this.time = time;
	}

	public int linkedVilage(int villageId) {
		if (villageId == village1) {
			return village2;
		}
		if (villageId == village2) {
			return village1;
		}
		return 0;
	}
}
```

---

#### My logic & feedback

#####  재귀를 이용한 풀이 로직 (테스트케이스 32번 시간초과)

1. 마을 클래스를 만든다 

2. 클래스는 1) 마을 id 2) ArrayList<도로object> 를 가진다.

3. 도로 클래스를 만든다. 
4. 클래스는 1) 도로지나는데 걸리는 시간과 연결마을 id 2개를 int로 가지며, 2)  parameter로 마을id를 넣으면 연결되어있는 다른 마을의 id를 int로 리턴하는 메소드를 가진다.

5. 1번 마을부터 '재귀'를 돈다. 마을에 연결된 도로 ArrayList를 메소드 파라미터로 넣어 연결된 상대 마을의 id를 얻어, 그 상대 마을로 다시 재귀를 호출한다.
6. 또 다른 마을로 갈 때 도로 object의 소요시간을 더한 값과 방문체크 결과도 함께 파라미터로 넘긴다.
7. 재귀 종료시점 : 더 이상 갈 수 있는 마을이 없거나 배달을 갔을 경우 걸리는 시간이 K보다 커질 때 종료된다.
8. 재귀가 끝나고 return되는 마을들의 HashMap을 전부 true로 replace한다.
9. 재귀가 다 끝나고 HashMap에서 true인 것의 개수가 정답이다.



위 풀이 로직은 재귀를 이용하여 31개의 테스트 케이스는 모두 통과하였으나, 마지막 32번째 테스트케이스는 시간초과로 통과하지 못한 로직이다. 

구글에 다른 사람들의 답변을 보니 '다익스트라 알고리즘'을 사용해서 풀었다는 후기가 많아서, 주말동안 해당 알고리즘의 내용을 공부해서 다시 풀어볼 계획이다.

 = > 5월 24일에 다익스트라 알고리즘을 써서 문제에 재도전했고, 테스트케이스를 모두 통과했다. 소스는 아래에.

 

##### 알고리즘 스터디 Feedback

스터디원 중 한명이 DFS를 Stack으로 돌 때 방문체크를 '마을'을 중심으로 하지 않고 '도로'를 기준으로 한다면 모든 마을을 다 방문할 수 있지 않을까 하는 아이디어를 내서, 이 방법으로 한번 시도해볼까 생각중이다.

그러나 문제는, 이미 재귀가 32번을 통과하지 못하여 Queue를 써서 BFS로 탐색하는 방법을 시도했었는데, 기존 재귀 로직과 소요시간이 큰 차이가 없어서 효과가 있을지는 잘 모르겠다.\

 = > 지환이와 스터디 도중 지환이에게 소스코드를 보여주었고, 지환이는 DFS를 돌리되 탐색 시 가장 짧은 경로(Road)만을 선택하는 방식으로 문제를 멋지게 풀었다. 즉  DFS나 BFS로 접근해도 문제를 풀 수 있으나, 내가 모든 경로를 탐색하는 방법으로 접근하여 마지막 테스트케이스에서 시간초과가 난 것 같다. 지환이에게 한 수 배웠다.



---

#### My first solution (Java) - Suceess (Dijkstra Algorithm)

2020.05.24 Try again and pass the test

```
class Solution {
    public int solution(int N, int[][] road, int K) {
        int answer = 0;
        int[][] arrDistance = new int[N + 1][N + 1];

        /* arrDistance 값 모두 2147483647로 초기화 */
        for (int i = 0; i < arrDistance.length; i++) {
            for (int j = 0; j < arrDistance.length; j++) {
                arrDistance[i][j] = Integer.MAX_VALUE;
            }
            arrDistance[i][i] = 0;
        }

        /* road의 값을 짧은 것만 찾아서 arrDistance에 넣어줌 */
        for (int i = 0; i < road.length; i++) {
            int village_1 = road[i][0];
            int village_2 = road[i][1];
            int distance = road[i][2];

            int value1 = arrDistance[village_1][village_2];
            if (distance < value1) {
                arrDistance[village_1][village_2] = distance;
            }
            int value2 = arrDistance[village_2][village_1];
            if (distance < value2) {
                arrDistance[village_2][village_1] = distance;
            }
        }
        int[] journey = arrDistance[1]; // 첫 행을 journey로 잡음
        int[] visitCheck = new int[journey.length];
        visitCheck[1] = 1;
        int cnt = 1;
        while (cnt < N) {
            //journey에서 거리가 가장 짧은 노드 찾음
            int maxValue = Integer.MAX_VALUE;
            int theClosestNode = Integer.MAX_VALUE;
            for (int i = 1; i < journey.length; i++) {
                if (visitCheck[i] == 0 && journey[i] < maxValue) {
                    theClosestNode = i;
                    maxValue = journey[i];
                }
            }
            visitCheck[theClosestNode] = 1;
            int nodeValue = journey[theClosestNode]; // 제일 짧은 노드까지 가는 distance
            /* journey에서 가장 짧은 노드를 기준으로 journey를 갱신 */
            for (int i = 1; i < arrDistance[theClosestNode].length; i++) {
                int journeyValue = journey[i];
                int comparedValue = arrDistance[theClosestNode][i];
                if (visitCheck[i] == 0 && comparedValue != Integer.MAX_VALUE && nodeValue + comparedValue < journeyValue) {
                    journey[i] = nodeValue + comparedValue;
                }
            }
            cnt++;
        }

        for (int i = 1; i < journey.length; i++) {
            if (journey[i] <= K) {
                answer++;
            }
        }
        return answer;
    }
}
```



---

#### My logic & feedback

##### 다익스트라 알고리즘으로 풀이

다익스트라 알고리즘은 한 정점에서 다른 모든 정점까지 가는 최단 경로를 찾는 알고리즘이다.

만약 노드 사이의 weight 가 주어져있지 않고, 단순히 노드 개수만으로 최단 경로를 찾는 문제라면(ex. 5x5 map에서의 최단경로) DFS나 BFS로 풀어도 무리가 없을 것이다.

그런데 만약 노드 사이의 거리가 weight로 각각 다르게 주어져 있다면? DFS나 BFS 풀이를 전개할 때 Distance를 따로 고려해주어야 하니 문제 풀이가 복잡해지게 된다.

이런 경우 2차원 배열로 '노드 간 거리를 갱신해나가는' 다익스트라 알고리즘을 활용하면 문제를 편하게 해결할 수 있다.

다익스트라 알고리즘 또한 하나의 '다이내믹 프로그래밍'이라 생각한다. 

다이내믹 프로그래밍이 그렇듯이, 해당 접근방식에 익숙하지 않다면 풀이방법을 생각해내기가...나같은 일반인의 수준에서는 굉장히 어려운 것 같다.

다익스트라 알고리즘의 내용은 따로 포스팅하는 것이 나을 것 같다.