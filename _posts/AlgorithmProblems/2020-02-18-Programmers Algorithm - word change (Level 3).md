---
layout: post
title: 프로그래머스 깊이/너비 우선탐색 - 단어 변환 (Level 3)
subtitle: Programmers Algorithm Test Solution
background: '/img/bg_technology.jpg'
categories: technology-algorithm

---

##### 문제 설명
두 개의 단어 begin, target과 단어의 집합 words가 있습니다. 아래와 같은 규칙을 이용하여 begin에서 target으로 변환하는 가장 짧은 변환 과정을 찾으려고 합니다.

1. 한 번에 한 개의 알파벳만 바꿀 수 있습니다.
2. words에 있는 단어로만 변환할 수 있습니다.
예를 들어 begin이 hit, target가 cog, words가 [hot,dot,dog,lot,log,cog]라면 hit -> hot -> dot -> dog -> cog와 같이 4단계를 거쳐 변환할 수 있습니다.

두 개의 단어 begin, target과 단어의 집합 words가 매개변수로 주어질 때, 최소 몇 단계의 과정을 거쳐 begin을 target으로 변환할 수 있는지 return 하도록 solution 함수를 작성해주세요.

##### 제한사항
각 단어는 알파벳 소문자로만 이루어져 있습니다.
각 단어의 길이는 3 이상 10 이하이며 모든 단어의 길이는 같습니다.
words에는 3개 이상 50개 이하의 단어가 있으며 중복되는 단어는 없습니다.
begin과 target은 같지 않습니다.
변환할 수 없는 경우에는 0를 return 합니다.
입출력 예
begin	target	words	return
hit	cog	[hot, dot, dog, lot, log, cog]	4
hit	cog	[hot, dot, dog, lot, log]	0

##### 입출력 예 설명
예제 #1
문제에 나온 예와 같습니다.

예제 #2
target인 cog는 words 안에 없기 때문에 변환할 수 없습니다.


출처 : https://programmers.co.kr/learn/courses/30/lessons/43163

---

#### My solution (Java)

```java
class Solution {
	ArrayList<Integer> answerList = new ArrayList<Integer>();

	public int solution(String begin, String target, String[] words) {
		int answer = 0;
		ArrayList<String> newWords = new ArrayList<String>();
		// ArrayList에 words 구성요소를 전부 다시 담는다(추후 remove하기 위함)
		for (String each : words) {
			newWords.add(each);
		}
		recursion(begin, target, newWords, 0);

		if (answerList.size() == 0) {
			return 0;
		} else {
			answer = answerList.get(0);
			for (int i = 1; i < answerList.size(); i++) {
				if (answerList.get(i) < answer) {
					answer = answerList.get(i);
				}
			}
		}
		return answer;
	}

	public void recursion(String begin, String target, ArrayList<String> newWords, int count) {
		if (begin.equals(target)) {
			answerList.add(count);
		}
		ArrayList<String> list = new ArrayList<String>();
		String[] beginSplit = begin.split("");
		for (int i = 0; i < newWords.size(); i++) {
			String[] wordSplit = newWords.get(i).split("");
			int check = 0;
			for (int j = 0; j < beginSplit.length; j++) {
				if (!beginSplit[j].equals(wordSplit[j])) {
					check++;
				}
			}
			if (check == 1) {
				list.add(newWords.get(i));
			}
		}
		for (int i = 0; i < list.size(); i++) {
			ArrayList<String> inputList = new ArrayList<String>();
			for (String each : newWords) {
				if (each.equals(list.get(i))) {
					continue;
				}
				inputList.add(each);
			}
			recursion(list.get(i), target, inputList, count + 1);
		}
	}
}

```

---

#### Feedback

```
처음에 재귀함수를 이용한 DFS 로 접근하였으나, 문제의 성질상 DFS보다는 BFS가 풀이에 더 용이하다는 것을 느꼈다. DFS로 해도 풀릴 것 같긴 하지만, 끝까지 내려갔다가 다시 올라오는 것 보다 넓이탐색을 우선 하고 답을 찾으면 바로 return하면 되기 때문이다(물론 의도대로 풀리진 않음..).
스택, 큐 등 자료구조를 모르다 보니 점점 이러한 탐색 문제 풀이에 있어 한계를 느끼는 중이다. 하루 빨리 자료구조를 따로 공부해서 알고리즘에 응용해야한다..

나의 logic
1) Words에 담긴 단어들을 for문으로 탐색하는데, 트리 구조에서 level이 낮아질 때 begin이 되는 단어는 words에서 삭제해야 하므로 String[]이 아닌 ArrayList<String> 이 낫다(remove 사용). 따라서 우선 words를 arrayList로 바꾸어준다.
2) words를 for문으로 탐색하며, 글자 1개만 다른 경우 이 놈들을 또 다른 ArrayList에 넣는다.
3) for문 탐색이 끝나면(넓이 탐색), 아까 넣었던 글자 1개만 다른 놈들을 대상으로 다시 재귀를 돌며 탐색한다.
4) 한 번 재귀를 돌 때마다 count + 1 을 하며, 얼마나 깊은 Level을 내려갔는지를 체크한다.
5) begin이 target과 일치하면 그 순간의 count를 answerList(ArrayList)에 담는다.
6) answerList에 담긴 숫자 중 가장 작은 숫자가 답이다.

나름대로의 BFS지만, 너무 비효율적인 탐색이 많이 들어갔기에 만약 효율성 테스트가 있었다면 100% 통과하지 못했을 것 같다. 하루 빨리 자료구조 및 탐색법을 익히자.
```

