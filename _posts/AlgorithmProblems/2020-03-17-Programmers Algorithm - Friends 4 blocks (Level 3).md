---
layout: post
title: 프로그래머스 프렌즈4블록 - 2018 카카오 블라인드 (Level 3)
subtitle: Algorithm Solution
background: '/img/bg_technology.jpg'
categories: technology/tech-algorithm
---


##### 문제 설명
프렌즈4블록
블라인드 공채를 통과한 신입 사원 라이언은 신규 게임 개발 업무를 맡게 되었다. 이번에 출시할 게임 제목은 프렌즈4블록.
같은 모양의 카카오프렌즈 블록이 2×2 형태로 4개가 붙어있을 경우 사라지면서 점수를 얻는 게임이다.

입력으로 블록의 첫 배치가 주어졌을 때, 지워지는 블록은 모두 몇 개인지 판단하는 프로그램을 제작하라.

##### 입력 형식
입력으로 판의 높이 m, 폭 n과 판의 배치 정보 board가 들어온다.
2 ≦ n, m ≦ 30
board는 길이 n인 문자열 m개의 배열로 주어진다. 블록을 나타내는 문자는 대문자 A에서 Z가 사용된다.
출력 형식
입력으로 주어진 판 정보를 가지고 몇 개의 블록이 지워질지 출력하라.

##### 입출력 예제
m	n	board	answer
4	5	[CCBDE, AAADE, AAABF, CCBBF]	14
6	6	[TTTANT, RRFACC, RRRFCC, TRRRAA, TTMMMF, TMMTTJ]	15
예제에 대한 설명
입출력 예제 1의 경우, 첫 번째에는 A 블록 6개가 지워지고, 두 번째에는 B 블록 4개와 C 블록 4개가 지워져, 모두 14개의 블록이 지워진다.
입출력 예제 2는 본문 설명에 있는 그림을 옮긴 것이다. 11개와 4개의 블록이 차례로 지워지며, 모두 15개의 블록이 지워진다.

(문제에 그림으로 설명하는 부분이 많으므로, 출처를 참고하여 직접 사이트에 들어가서 확인 요망)

출처 : https://programmers.co.kr/learn/courses/30/lessons/17679

---



#### My solution (Java)

```java
import java.util.ArrayList;
import java.util.HashMap;

class Solution {
 	ArrayList<Block> list = new ArrayList<Block>();
	public int solution(int m, int n, String[] board) {
		int answer = 0;
		int id = 0;
		int eachRow = 0;
		for (int i = 0; i < m; i++) {
			String[] eachLetter = board[eachRow].split("");
			int getLetter = 0;
			for (int j = 0; j < n; j++) {
				Block block = new Block(id, eachLetter[getLetter], i, j);
				list.add(block);
				id++;
				getLetter++;
			}
			eachRow++;
		}

		while (true) {
			/* 4 blocks를 탐색한다 */
			HashMap<Integer, Boolean> fourMap = fourBlockSearch(m, n, board);
			if (fourMap.get(-1) == false) {
				break;
			}
			/* fourMap대로 letter 변경 */
			id = 0;
			for (int i = 0; i < m; i++) {
				for (int j = 0; j < n; j++) {
					if (fourMap.get(id) == true) {
						list.get(id).letter = "x";
					}
					id++;
				}
			}

			/* x로 입력되어있는 아래 줄 라인의 id를 lowerLineSearch에 담는다 */
			HashMap<Integer, Integer> lowerLineMap = new HashMap<Integer, Integer>();
			lowerLineMap = lowerLineSearch(m, n, board);

			/* 각 Column마다 새로운 값을 넣어 줄 switchTarget list를 만든다 */
			ArrayList<ArrayList<String>> switchTarget = new ArrayList<ArrayList<String>>();
			for (int i = 0; i < n; i++) {
				switchTarget.add(insertUpperLetters(i, lowerLineMap, m, n, board));
			}

			/*
			 * lowerLineMap을 탐색하면서, switchTarget의 값을 맨 아래줄 x부터 각 switchTarget의 list size만큼
			 * 담아준다
			 */
			switchLetter(lowerLineMap, switchTarget, m, n);
		}
		/* 전체 블록의 x의 개수를 헤아려준다 */
		for (int i = 0; i < list.size(); i++) {
			if (list.get(i).letter.equals("x")) {
				answer++;
			}
		}
		return answer;

	}

	public void switchLetter(HashMap<Integer, Integer> lowerLineMap, ArrayList<ArrayList<String>> switchTarget, int m,
			int n) {
		for (int i = 0; i < n; i++) {
			int start = lowerLineMap.get(i);
			if (start < 0) {
				continue;
			}
			for (int j = 0; j < switchTarget.get(i).size(); j++) {
				list.get(start).letter = switchTarget.get(i).get(j);
				start -= n;
			}
		}
	}

	/* 2x2 block을 탐색하는 메소드 */
	public HashMap<Integer, Boolean> fourBlockSearch(int m, int n, String[] board) {
		HashMap<Integer, Boolean> fourMap = new HashMap<Integer, Boolean>();
		/* 2x2블록이 아무것도 없으면 key = -1 일 때 값은 false다. */
		fourMap.put(-1, false);
		for (int i = 0; i < list.size(); i++) {
			fourMap.put(i, false);
		}
		for (int i = 0; i < list.size(); i++) {
			Block block = list.get(i);
			int count = 0;
			/* 오른쪽 탐색 */
			if (block.y < n - 1) {
				/* blcok의 letter 값이 x일때는 탐색하지 않는다 */
				if (list.get(i + 1).letter.equals("x")) {
					continue;
				}
				if (list.get(i + 1).letter.equals(block.letter)) {
					count++;
				}
			}
			/* 아래쪽 탐색 */
			if (block.x < m - 1) {
				if (list.get(i + n).letter.equals(block.letter)) {
					count++;
				}
			}
			/* 대각선 탐색 */
			if (block.y < n - 1 && block.x < m - 1) {
				if (list.get(i + n + 1).letter.equals(block.letter)) {
					count++;
				}
			}
			if (count == 3) {
				fourMap.replace(i, true);
				fourMap.replace(i + 1, true);
				fourMap.replace(i + n, true);
				fourMap.replace(i + n + 1, true);
				fourMap.replace(-1, true);
			}
		}
		return fourMap;
	}

	/* 각 컬럼의 가장 아래쪽 x의 block id를 찾아서 HashMap으로 반환 */
	public HashMap<Integer, Integer> lowerLineSearch(int m, int n, String[] board) {
		HashMap<Integer, Integer> lowerLineMap = new HashMap<Integer, Integer>();
		for (int i = 0; i < n; i++) {
			for (int j = m * n - n + i; j >= 0; j -= n) {
				Block block = list.get(j);
				if (block.letter.equals("x")) {
					lowerLineMap.put(i, block.id);
					break;
				}
				lowerLineMap.put(i, -1);
			}
		}
		return lowerLineMap;
	}

	/* 각 컬럼을 세로로 가장 아래쪽 x부터 탐색하면서, 문자를 list에 저장함과 동시에 x로 변환해줌 */
	public ArrayList<String> insertUpperLetters(int column, HashMap<Integer, Integer> lowerLineMap, int m, int n,
			String[] board) {
		ArrayList<String> listOfNotX = new ArrayList<String>();

		for (int i = lowerLineMap.get(column); i >= 0; i -= n) {
			if (!list.get(i).letter.equals("x")) {
				listOfNotX.add(list.get(i).letter);
				list.get(i).letter = "x";
			}
		}
		return listOfNotX;
	}

}

class Block {
	int id;
	String letter;
	int x;
	int y;

	public Block(int id, String letter, int x, int y) {
		this.id = id;
		this.letter = letter;
		this.x = x;
		this.y = y;
	}
}
```



---



#### Feedback

```
그래프 문제의 업그레이드다. 문제 자체는 어렵지 않으나 코드량이 많다. 
For문과 List, HashMap을 적당히 사용해서 0(N^2)으로 답이 나왔는데,
list가 아니라 Queue를 사용해서 문자를 담은 후 그냥 출력해주기만 하면 더 빠를 것 같다. 추후 시간이 나면 코드를 수정해보아야 겠다.

나의 logic
1. 각 블록을 Class로 만들고 객체를 설정하여 x,y좌표 및 고유 문자를 담는다.
2. 전체 탐색을 하는데, 한 블록을 기준으로 오른쪽, 아래, 대각선을 탐색하여 고유 문자가 같으면 문자를 x로 만든다.
3. 탐색이 끝나면, 각 세로줄마다 가장 아래쪽 x의 index를 Map에 저장한다.
4. 각 세로줄마다 가장 아래쪽 x의 index에서 위쪽으로 탐색을 하면서, x가 아닌 문자들을 list에 저장함과 동시에 x로 만든다.
5. 다시 세로줄 탐색을 하면서, list에 저장되어 있는 놈들을 아래쪽의 x와 바꾸면서 올라간다.
6. 2~5의 과정을 반복한다.
```

