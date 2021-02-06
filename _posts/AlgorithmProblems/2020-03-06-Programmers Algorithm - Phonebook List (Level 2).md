---
layout: post
title: 프로그래머스 해시 - 전화번호 목록 (Level 2)
subtitle: Programmers Algorithm Test Solution
background: '/img/bg_technology.jpg'
categories: technology/tech-algorithm
---


##### 문제 설명
전화번호부에 적힌 전화번호 중, 한 번호가 다른 번호의 접두어인 경우가 있는지 확인하려 합니다.
전화번호가 다음과 같을 경우, 구조대 전화번호는 영석이의 전화번호의 접두사입니다.

구조대 : 119
박준영 : 97 674 223
지영석 : 11 9552 4421
전화번호부에 적힌 전화번호를 담은 배열 phone_book 이 solution 함수의 매개변수로 주어질 때, 어떤 번호가 다른 번호의 접두어인 경우가 있으면 false를 그렇지 않으면 true를 return 하도록 solution 함수를 작성해주세요.

##### 제한 사항
phone_book의 길이는 1 이상 1,000,000 이하입니다.
각 전화번호의 길이는 1 이상 20 이하입니다.
입출력 예제
phone_book	return
[119, 97674223, 1195524421]	false
[123,456,789]	true
[12,123,1235,567,88]	false

##### 입출력 예 설명

##### 입출력 예 #1
앞에서 설명한 예와 같습니다.

##### 입출력 예 #2
한 번호가 다른 번호의 접두사인 경우가 없으므로, 답은 true입니다.

##### 입출력 예 #3
첫 번째 전화번호, “12”가 두 번째 전화번호 “123”의 접두사입니다. 따라서 답은 false입니다.

출처 : https://programmers.co.kr/learn/courses/30/lessons/42577

---

#### My solution (Java)

```
class Solution {
	public boolean solution(String[] phone_book) {
		boolean answer = true;
		String sum = "";
		/* 모든 문자열을 합쳐 sum을 만듬*/
		for (String each : phone_book) {
			sum += "/" + each;
		}
		/* sum에서 각 phone_book String의 index 탐색, 비교*/
		for (int i = 0; i < phone_book.length; i++) {
			int a = sum.indexOf("/" + phone_book[i]);
		int b = sum.lastIndexOf("/" + phone_book[i]);
			if (a != b) {
				answer = false;
				break;
			}
		}
		return answer;
	}
}
```



---

#### Feedback

```
분류가 해쉬길래 맵을 사용하려 했는데, 더 좋은 방법이 떠올라 그냥 그 방법으로 풀었다. 해쉬로는 어떻게 하라는건지 잘 모르겠음.

나의 Logic
1. 모든 phone_book의 문자열을 합쳐 sum을 만드는데, 합칠 때 "/"기호를 넣는다. 추후 index로 탐색을 할 때 "/"이 문자열 맨 앞에 붙어있을 테니까 접두어인지 아닌지 구분이 된다.
2. 탐색을 하면서 index, lastIndex를 각각 찾는데, 만약 두 값이 다르다면 같은 접두어가 2번 들어있다는 의미이므로 false를 반환한다.
3. 아니라면 true를 반환한다.
```
