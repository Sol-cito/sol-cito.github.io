---
layout: post
title: 프로그래머스 자물쇠와 열쇠 - 2020 카카오 블라인드 (Level 3)
subtitle: Algorithm Solution
background: '/img/bg_technology.jpg'
categories: technology-algorithm

---

##### 문제 설명
고고학자인 튜브는 고대 유적지에서 보물과 유적이 가득할 것으로 추정되는 비밀의 문을 발견하였습니다. 그런데 문을 열려고 살펴보니 특이한 형태의 자물쇠로 잠겨 있었고 문 앞에는 특이한 형태의 열쇠와 함께 자물쇠를 푸는 방법에 대해 다음과 같이 설명해 주는 종이가 발견되었습니다.

잠겨있는 자물쇠는 격자 한 칸의 크기가 1 x 1인 N x N 크기의 정사각 격자 형태이고 특이한 모양의 열쇠는 M x M 크기인 정사각 격자 형태로 되어 있습니다.

자물쇠에는 홈이 파여 있고 열쇠 또한 홈과 돌기 부분이 있습니다. 열쇠는 회전과 이동이 가능하며 열쇠의 돌기 부분을 자물쇠의 홈 부분에 딱 맞게 채우면 자물쇠가 열리게 되는 구조입니다. 자물쇠 영역을 벗어난 부분에 있는 열쇠의 홈과 돌기는 자물쇠를 여는 데 영향을 주지 않지만, 자물쇠 영역 내에서는 열쇠의 돌기 부분과 자물쇠의 홈 부분이 정확히 일치해야 하며 열쇠의 돌기와 자물쇠의 돌기가 만나서는 안됩니다. 또한 자물쇠의 모든 홈을 채워 비어있는 곳이 없어야 자물쇠를 열 수 있습니다.

열쇠를 나타내는 2차원 배열 key와 자물쇠를 나타내는 2차원 배열 lock이 매개변수로 주어질 때, 열쇠로 자물쇠를 열수 있으면 true를, 열 수 없으면 false를 return 하도록 solution 함수를 완성해주세요.

##### 제한사항
key는 M x M(3 ≤ M ≤ 20, M은 자연수)크기 2차원 배열입니다.
lock은 N x N(3 ≤ N ≤ 20, N은 자연수)크기 2차원 배열입니다.
M은 항상 N 이하입니다.
key와 lock의 원소는 0 또는 1로 이루어져 있습니다.
0은 홈 부분, 1은 돌기 부분을 나타냅니다.

입출력 예
key									lock								result
[[0, 0, 0], [1, 0, 0], [0, 1, 1]]	[[1, 1, 1], [1, 1, 0], [1, 0, 1]]	true

key를 시계 방향으로 90도 회전하고, 오른쪽으로 한 칸, 아래로 한 칸 이동하면 lock의 홈 부분을 정확히 모두 채울 수 있습니다.


출처 : https://programmers.co.kr/learn/courses/30/lessons/60059



---

#### My solution (Java)

```java
class Solution {
	public boolean solution(int[][] key, int[][] lock) {
		boolean answer = true;
		int lockLength = lock.length;
		int keyLength = key.length;
		int addLength = keyLength - 1; // lock 2차원 배열을 얼마만큼 확장할 것인지
		int newLength = lockLength + 2 * addLength; // 새로만들 확장할 배열의 길이

		/* 기존 lock 배열의 값을 list에 저장 */
		int[] list = new int[lockLength * lockLength];
		int count = 0;
		for (int i = 0; i < lockLength; i++) {
			for (int j = 0; j < lockLength; j++) {
				list[count] = lock[i][j];
				count++;
			}
		}

		/* 기존 key 배열의 값을 list에 저장 */
		int[] list_key = new int[keyLength * keyLength];
		int count_key = 0;
		for (int i = 0; i < keyLength; i++) {
			for (int j = 0; j < keyLength; j++) {
				list_key[count_key] = key[i][j];
				count_key++;
			}
		}
		/* 새로 만들 확장 배열의 값을 0으로 초기화 */
		int[][] newArray = new int[newLength][newLength];
		for (int i = 0; i < newLength; i++) {
			for (int j = 0; j < newLength; j++) {
				newArray[i][j] = 0;
			}
		}
		/* 기존 배열의 값을 새로운 위치(addLength만큼 늘어난 위치)에 넣기 */
		count = 0;
		for (int i = addLength; i < addLength + lockLength; i++) {
			for (int j = addLength; j < addLength + lockLength; j++) {
				newArray[i][j] = list[count];
				count++;
			}
		}

		/* key 배열을 위에서부터 차례대로 더하며 탐색한다 */
		for (int x = 0; x < 5; x++) {
			answer = search(addLength, keyLength, lockLength, newArray, list_key);
			if (answer == false) {
				key = keyTurning(key);
				count_key = 0;
				for (int i = 0; i < keyLength; i++) {
					for (int j = 0; j < keyLength; j++) {
						list_key[count_key] = key[i][j];
						count_key++;
					}
				}
			} else {
				break;
			}
		}

		return answer;
	}

	public int[][] keyTurning(int[][] key) { // key를 오른쪽으로 90도 회전
		int n = key.length;
		int[][] newArray = new int[n][n];

		for (int i = 0; i < n; i++) {
			int right = (n - 1) - i;
			for (int j = 0; j < n; j++) {
				newArray[j][right] = key[i][j];
			}
		}
		return newArray;
	}

	public boolean validation(int[][] result, int lockLength, int addLength) {
		for (int i = addLength; i < lockLength + addLength; i++) {
			for (int j = addLength; j < lockLength + addLength; j++) {
				if (result[i][j] != 1) {
					return false;
				}
			}
		}
		return true;
	}

	public int[][] createTestArray(int[][] newArray) {
		int n = newArray.length;
		int[][] testArray = new int[n][n];
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < n; j++) {
				testArray[i][j] = newArray[i][j];
			}
		}
		return testArray;
	}

	public boolean search(int addLength, int keyLength, int lockLength, int[][] newArray, int[] list_key) {
		int start_i = 0;
		int start_j = 0;
		while (true) {
			if (start_i == addLength + lockLength) {
				start_i = 0;
				start_j++;
			}
			if (start_j == addLength + lockLength) {
				break;
			}
			int[][] testArray = createTestArray(newArray);

			int test_count = 0;
			for (int i = start_i; i < start_i + keyLength; i++) {
				for (int j = start_j; j < start_j + keyLength; j++) {
					testArray[i][j] += list_key[test_count];
					test_count++;
				}
			}

			if (validation(testArray, lockLength, addLength) == true) {
				return true;
			} else {
				start_i++;
			}
		}
		return false;
	}
}
```



---

#### Feedback

```
반성해야 될 부분 : 카카오 공채 문제고, 어려워 보여서 혹시 내가 알고있지 않은 알고리즘이나 자료구조로 푸는 것이 아닐까 하는 생각을 했다. 결국 조금 고민하다가 다른 사람의 풀이를 보고 '2차원 배열을 잘 쓰면 된다'는 말을 보고 풀이를 시작했는데, 2차원 배열과 반복문으로도 충분히 풀 수 있는 문제였다. 앞으로는 문제에 쫄지말고 고민을 좀 더 많이 해보는 습관을 길러야 겠다.

풀이 로직은 아래와 같다.
1) key 배열을 90도 돌리는 함수를 따로 만든다.
2) 기존 lock 배열을 (key - 1)만큼 확장한다.
3) key 배열을 확장된 lock 배열을 따라 완전 탐색하며 배열의 각 요소와 더한다.
4) 가운데 lock 배열 요소가 모두 1이면 탐색을 멈추고 true를 반환한다.
5) 완전 탐색을 했는데도 결과가 false면 key 배열을 90도 돌려서 한 번 더 수행한다.
6) 90도 돌리는 함수가 4번 실행되었는데도 false면 false를 반환한다.

반복문이 많이 들어가 시간 초과가 나지 않을까 지레 겁먹었었는데, 사실 생각해보니 내가 에전에 풀었던 더 단순한 문제들도 이중 포문이나 와일문을 많이 썼었는데도 시간초과가 나지 않았었다. 

쫄지말고 문제에 도전하는 습관을 기르자.
```

