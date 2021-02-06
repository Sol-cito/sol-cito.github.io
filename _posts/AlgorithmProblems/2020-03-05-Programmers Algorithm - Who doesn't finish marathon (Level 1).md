---
layout: post
title: 프로그래머스 해시 - 완주하지 못한 선수 (Level 1)
subtitle: Algorithm Solution
background: '/img/bg_technology.jpg'
categories: technology/tech-algorithm
---


##### 문제 설명
수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

##### 제한사항
마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
completion의 길이는 participant의 길이보다 1 작습니다.
참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
참가자 중에는 동명이인이 있을 수 있습니다.
입출력 예
participant	completion	return
[leo, kiki, eden]	[eden, kiki]	leo
[marina, josipa, nikola, vinko, filipa]	[josipa, filipa, marina, nikola]	vinko
[mislav, stanko, mislav, ana]	[stanko, ana, mislav]	mislav

##### 입출력 예 설명
예제 #1
leo는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #2
vinko는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #3
mislav는 참여자 명단에는 두 명이 있지만, 완주자 명단에는 한 명밖에 없기 때문에 한명은 완주하지 못했습니다.
출처 : https://programmers.co.kr/learn/courses/30/lessons/42576


---

#### My solution (Java)

```
import java.util.HashMap;
class Solution {
	public String solution(String[] participant, String[] completion) {
		String answer = "";
		HashMap<String, Integer> map = new HashMap<String, Integer>();
		for (int i = 0; i < completion.length; i++) {
			if (map.get(completion[i]) == null) {
				map.put(completion[i], 1);
			} else {
				int x = map.get(completion[i]);
				map.replace(completion[i], x + 1);
			}
		}

		for (int i = 0; i < participant.length; i++) {
			if ((map.get(participant[i]) == null) || map.get(participant[i]) == 0) {
				answer = participant[i];
				break;
			}
			if (map.get(participant[i]) > 0) {
				int x = map.get(participant[i]);
				map.replace(participant[i], x - 1);
				continue;
			}
		}
		return answer;
	}
}
```



---

#### Feedback

```
Level1 문제 답게 해시의 정석같은 문제. 다른 사람들의 풀이를 보니 파라미터로 주어진 두 개의 String[]을 정렬 한 후 어찌어찌해서 풀었던데, 그럼 결국 포문을 2번 돌고나서 탐색을 시작하기 때문에 해시보다 효율성이 떨어지지 않나..하고 생각했다.
처음에 동명이인을 고려하지 않고 풀었다가 오답처리되어서 당황했고, 코드를 수정하고 통과했다. 문제를 끝까지 잘 읽자.

나의 Logic
1. completion[]을 for문으로 돌면서 hash value에 1을 넣어주는데, 동명이인이 있어서 중복으로 들어오면 기존 값에 1을 더한다.
2. participant[]를 for문으로 돌면서 vlaue를 체크하는데, value가 0보다 크면 value - 1 을 한다. 
3. 이 논리라면 participant[]를 돌 때 완주하지 못한 사람의 value는 0이거나(이미 동명이인이 있어서 그 전에 -1을 했으므로), null(동명이인은 없지만 completion[]에 있지 않아 value를 부여받지 못함)이 될 것이다.
```
