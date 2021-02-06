---
layout: post
title: 프로그래머스 가사검색 - 2020카카오공채 (Level 4) (효율성 3개 통과 못함)
subtitle: Programmers Algorithm Test Solution
background: '/img/bg_technology.jpg'
categories: technology/tech-algorithm
---


##### 문제 설명
[본 문제는 정확성과 효율성 테스트 각각 점수가 있는 문제입니다.]

친구들로부터 천재 프로그래머로 불리는 프로도는 음악을 하는 친구로부터 자신이 좋아하는 노래 가사에 사용된 단어들 중에 특정 키워드가 몇 개 포함되어 있는지 궁금하니 프로그램으로 개발해 달라는 제안을 받았습니다.
그 제안 사항 중, 키워드는 와일드카드 문자중 하나인 '?'가 포함된 패턴 형태의 문자열을 뜻합니다. 와일드카드 문자인 '?'는 글자 하나를 의미하며, 어떤 문자에도 매치된다고 가정합니다. 예를 들어 "fro??"는 "frodo", "front", "frost" 등에 매치되지만 "frame", "frozen"에는 매치되지 않습니다.

가사에 사용된 모든 단어들이 담긴 배열 words와 찾고자 하는 키워드가 담긴 배열 queries가 주어질 때, 각 키워드 별로 매치된 단어가 몇 개인지 순서대로 배열에 담아 반환하도록 solution 함수를 완성해 주세요.

##### 가사 단어 제한사항
words의 길이(가사 단어의 개수)는 2 이상 100,000 이하입니다.
각 가사 단어의 길이는 1 이상 10,000 이하로 빈 문자열인 경우는 없습니다.
전체 가사 단어 길이의 합은 2 이상 1,000,000 이하입니다.
가사에 동일 단어가 여러 번 나올 경우 중복을 제거하고 words에는 하나로만 제공됩니다.
각 가사 단어는 오직 알파벳 소문자로만 구성되어 있으며, 특수문자나 숫자는 포함하지 않는 것으로 가정합니다.
검색 키워드 제한사항
queries의 길이(검색 키워드 개수)는 2 이상 100,000 이하입니다.
각 검색 키워드의 길이는 1 이상 10,000 이하로 빈 문자열인 경우는 없습니다.
전체 검색 키워드 길이의 합은 2 이상 1,000,000 이하입니다.
검색 키워드는 중복될 수도 있습니다.
각 검색 키워드는 오직 알파벳 소문자와 와일드카드 문자인 '?' 로만 구성되어 있으며, 특수문자나 숫자는 포함하지 않는 것으로 가정합니다.
검색 키워드는 와일드카드 문자인 '?'가 하나 이상 포함돼 있으며, '?'는 각 검색 키워드의 접두사 아니면 접미사 중 하나로만 주어집니다.
예를 들어 "??odo", "fro??", "?????"는 가능한 키워드입니다.
반면에 "frodo"('?'가 없음), "fr?do"('?'가 중간에 있음), "?ro??"('?'가 양쪽에 있음)는 불가능한 키워드입니다.

#####  입출력 예
words	queries	result
["frodo", "front", "frost", "frozen", "frame", "kakao"]	["fro??", "????o", "fr???", "fro???", "pro?"]	[3, 2, 4, 1, 0]
입출력 예에 대한 설명
"fro??"는 "frodo", "front", "frost"에 매치되므로 3입니다.
"????o"는 "frodo", "kakao"에 매치되므로 2입니다.
"fr???"는 "frodo", "front", "frost", "frame"에 매치되므로 4입니다.
"fro???"는 "frozen"에 매치되므로 1입니다.
"pro?"는 매치되는 가사 단어가 없으므로 0 입니다.


출처 : https://programmers.co.kr/learn/courses/30/lessons/60060

---

#### My solution (Java) - 효율성 테스트 5개중 3개를 통과하지 못함

```java
import java.util.HashMap;

class Solution {
	public int[] solution(String[] words, String[] queries) {
		int[] answer = new int[queries.length];
		HashMap<Integer, String> map = new HashMap<Integer, String>();

		for (int i = 0; i < queries.length; i++) {
			int firstL = queries[i].indexOf("?");
			int lastL = queries[i].lastIndexOf("?");
			if (firstL == 0) { // querie가 ?로 시작할 때
				firstL = lastL + 1;
				lastL = queries[i].length() - 1;
			} else if (firstL != 0) { // querie가 ?으로 시작 안할 때(뒤에 있을 때)
				lastL = firstL - 1;
				firstL = 0;
			}
			if (firstL > lastL) {
				String value = "0/" + (queries[i].length() - 1);
				map.put(i, value);
			} else {
				String value = firstL + "/" + lastL;
				map.put(i, value);
			}
		}

		for (int i = 0; i < queries.length; i++) {
			int count = 0;
			String[] chunk = map.get(i).split("/");

			int from = Integer.parseInt(chunk[0]);
			int to = Integer.parseInt(chunk[1]);
			String queries_subString = queries[i].substring(from, to + 1);

			for (int j = 0; j < words.length; j++) {
				if (queries[i].length() != words[j].length()) {
					continue;
				}
				String words_subString = words[j].substring(from, to + 1);
				if (words_subString.equals(queries_subString)) {
					count++;
				}
			}
			answer[i] = count;
		}
		return answer;
	}
}
```

---

#### Feedback

```
테스트 케이스는 모두 통과하였으나, 효율성 테스트 5개 중 3개를 통과하지 못하였다..(input값이 클 때 속도가 안나온다는 뜻)

문제 자체의 풀이는 쉬운 편이나, 이 문제가 카카오 공채인 이유는 바로 '효율성 테스트'를 통과해야 하기 때문인 것 같다.

사실 처음 풀었을 때는 효율성 테스트 5개를 모두 통과하지 못하였다. 3중 for문을 써서 시간복잡도가 O(n^3)이 되었기 때문이다(테스트케이스는 통과함). 그래서 for문을 3개에서 2개로 줄이고자 노력했고, String 내장함수인 indexOf 및 lastIndexOf 를 써서 for문을 2개로 줄이는 데 성공했다. 로직은, 

1) 각각의 Querie에서 "?"를 제외한 나머지 글자의 index를 indexOf와 lastIndexOf로 구하여 "3/5" 와 같은 String의 형태로 만들어 HashMap에 저장한다(Key : Querie배열의 index를 int로 저장, Value : "3/5"와 같은 형태로 만든 String을 저장)
2) for문으로 HashMap의 Key를 돌면서 Value를 Split으로 자르고, 그 결과값(index 2개)으로 각 단어를 subString한다.
3) 이 때 만약 단어의 길이와 Querie의 길이가 다르면 넘긴다.
4) 길이가 같고 subString의 결과값이 Querie와 equal하면, count++ 를 한 후 answer배열에 담는다.
5) answer 배열을 return한다.

이렇게 내장함수로 for문을 두 번 쓴 후, 테스트 케이스를 돌렸을 때 시간이 비약적으로 줄어드는 것을 확인(1000ms에서 100ms로 거의 10배 줄어들었다)하였으나, 더 어마어마한 input이 들어오는 효율성테스트를 결국 3개 통과하지 못하였다.

다른 사람들이 올린 글을 보니, 애초에 이 문제는 for문과 같이 선형적으로 접근해서는 효율성 테스트를 통과할 수 없으며, Tree와 같은 자료구조로 접근해야 풀 수 있다고 한다.

나는 아직 그 영역을 배워본 적이 없기 때문에, 목요일 스터디에서 이 내용을 스터디원들에게 물어볼 참이다.

```
---
#### 스터디 Feedback

스터디원들과 머리를 맞대고 구글링을 해보고 답을 살펴본 결과, 이 문제는 **Trie** 라는 특별한 트리구조를 써야만 O(N)의 시간복잡도로 효율성 테스트를 통과할 수 있는 문제라고 한다...
Trie는, 트리구조가 있을 때 노드 하나에 '한 단어'만을 담고 그 철자 하나씩 탐색하며 하위 노드로 진행하는, 그런 특수한 트리구조다.
사실 그 트리구조 자체는 이해가긴하는데, trie를 '어떻게' 구현하는지는 또 다른 문제다.
팀원들과 '트라이'가 아니라 '또라이'라고 이름붙인 이 알고리즘...나중에 반드시 다시 한번 도전해서 정복해야겠다.
