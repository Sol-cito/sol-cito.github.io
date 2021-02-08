---
layout: post
title: 프로그래머스 정렬  - H-index (Level 2)
subtitle: Algorithm Solution
background: '/img/bg_technology.jpg'
categories: technology-algorithm

---


###### 문제 설명
H-Index는 과학자의 생산성과 영향력을 나타내는 지표입니다. 어느 과학자의 H-Index를 나타내는 값인 h를 구하려고 합니다. 위키백과1에 따르면, H-Index는 다음과 같이 구합니다.

어떤 과학자가 발표한 논문 n편 중, h번 이상 인용된 논문이 h편 이상이고 나머지 논문이 h번 이하 인용되었다면 h가 이 과학자의 H-Index입니다.

어떤 과학자가 발표한 논문의 인용 횟수를 담은 배열 citations가 매개변수로 주어질 때, 이 과학자의 H-Index를 return 하도록 solution 함수를 작성해주세요.

###### 제한사항
과학자가 발표한 논문의 수는 1편 이상 1,000편 이하입니다.
논문별 인용 횟수는 0회 이상 10,000회 이하입니다.

입출력 예
heights				return
[6,9,5,7,4]			[0,0,2,2,4]
[3,9,9,3,5,7,2]		[0,0,0,3,3,3,6]
[1,5,3,6,7,6,5]		[0,0,2,0,0,5,6]

###### 입출력 예
citations			return
[3, 0, 6, 1, 5]		3

입출력 예 설명
이 과학자가 발표한 논문의 수는 5편이고, 그중 3편의 논문은 3회 이상 인용되었습니다. 그리고 나머지 2편의 논문은 3회 이하 인용되었기 때문에 이 과학자의 H-Index는 3입니다.

https://en.wikipedia.org/wiki/H-index 위키백과 참고

출처 : https://programmers.co.kr/learn/courses/30/lessons/42747



---

#### My solution (Java)

```java
class Solution {
	public int solution(int[] citations) {
		int answer = 0;
		int length = citations.length;
		for (int i = 1; i <= length; i++) {
			int count = 0;
			for (int j = 0; j < length; j++) {
				if(citations[j] >= i) {
					count++;
				}
			}
			if(count >= i && count >= answer) {
				answer = i;
			}
		}
		return answer;
	}
}
```



---

#### Feedback

```
문제가 알아듣기 어렵게 제시되어있다.
만약 문제가, "어떤 연구자가 h번 이상 인용된 h개 이상의 논문을 발표했다면, h의 최대값은 얼마인가?"
라고 되어있었으면 훨씬 더 이해하기 쉬웠을 것 같다.

풀이는 굉장히 단순했는데, 사고과정은 다음과 같다.

1. 배열이 들어온다.
2. 배열의 Length를 구한다.
3. For문으로 1 - Length 까지 돌면서 h를 검증한다.
 - 'i개의 논문이 i번 이상 인용되었는가? (Yes -> count++)'
 - count를 answer값에 저장한다.
4. For문 한번 반복할 때마다 최대값을 구하기 위해 새로 들어온 count값과 answer값을 비교한다.

< Extra test cases>
[10, 8, 5, 4, 3] -> out : 4
[25, 8, 5, 3, 3] -> out : 3
[22, 42] -> out : 2
```

