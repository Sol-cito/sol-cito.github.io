---
layout: post
title: 프로그래머스 탐욕법 - 구명보트 (Level 2)
subtitle: Programmers Algorithm Test Solution
background: '/img/bg_technology.jpg'
categories: technology
---

```
문제 설명
무인도에 갇힌 사람들을 구명보트를 이용하여 구출하려고 합니다. 구명보트는 작아서 한 번에 최대 2명씩 밖에 탈 수 없고, 무게 제한도 있습니다.

예를 들어, 사람들의 몸무게가 [70kg, 50kg, 80kg, 50kg]이고 구명보트의 무게 제한이 100kg이라면 2번째 사람과 4번째 사람은 같이 탈 수 있지만 1번째 사람과 3번째 사람의 무게의 합은 150kg이므로 구명보트의 무게 제한을 초과하여 같이 탈 수 없습니다.

구명보트를 최대한 적게 사용하여 모든 사람을 구출하려고 합니다.

사람들의 몸무게를 담은 배열 people과 구명보트의 무게 제한 limit가 매개변수로 주어질 때, 모든 사람을 구출하기 위해 필요한 구명보트 개수의 최솟값을 return 하도록 solution 함수를 작성해주세요.

제한사항
무인도에 갇힌 사람은 1명 이상 50,000명 이하입니다.
각 사람의 몸무게는 40kg 이상 240kg 이하입니다.
구명보트의 무게 제한은 40kg 이상 240kg 이하입니다.
구명보트의 무게 제한은 항상 사람들의 몸무게 중 최댓값보다 크게 주어지므로 사람들을 구출할 수 없는 경우는 없습니다.
입출력 예
people	limit	return
[70, 50, 80, 50]	100	3
[70, 80, 50]	100	3

출처 : https://programmers.co.kr/learn/courses/30/lessons/42885?language=java
```
---


#### My solution (Java)

```java
class Sol_boat {
	int count = 0;

	public int solution(int[] people, int limit) {
		int answer = 0;
		Arrays.sort(people);
		ArrayList<Integer> list = new ArrayList<Integer>();

		for (int i = people.length - 1; i >= 0; i--) {
			list.add(people[i]);
		}

		greedy(list, limit);
		answer = count;
		return answer;
	}

	public void greedy(ArrayList<Integer> list, int limit) {
		/* 
		 * 모든 case에서 size == 0 이면 Greedy 재귀 돌기 전에 return
		 */
		/* 탐색할 리스트 길이가 1이면 count 하고 재귀 종료 */
		if (list.size() == 1) {
			count++;
			return;
		}
		/* 해당 리스트의 첫 수가 limit과 같으면 count하고 인덱스 0을 삭제 후 greedy 재귀 */
		if (list.get(0) == limit) {
			count++;
			list.remove(0);
			if (list.size() == 0) {
				return;
			}
			greedy(list, limit);
			return;
		}
		/* 인덱스0부터 전부 탐색했는데 같이 태울 사람 있으면 count++, 인덱스 i, 0 을 삭제 후 greedy 재귀 */
		for (int i = 0; i < list.size(); i++) {
			if (list.get(0) + list.get(i) <= limit) {
				count++;
				list.remove(i);
				list.remove(0);
				if (list.size() == 0) {
					return;
				}
				greedy(list, limit);
				return;
			}
		}
		if (list.size() == 0) {
			return;
		}
		/* 탐색 다 했는데도 같이 탈 사람 없으면 count++, 인덱스 0을 remove하고 greedy 재귀 */
		count++;
		list.remove(0);
		greedy(list, limit);
	}
}
```

---

#### My logic

```
위 주석에 나와있듯이, 나의 첫 접근방식은 케이스를 4가지로 나누어서 각 케이스별로 재귀를 돌리던가, 리턴을 하던가 하는 방법이었다.
그러나 이 경우 효율성테스트에서 모두 시간초과가 나는 문제가 발생했다.
시간을 줄일 수 있는 방법을 찾아보고자 한다.
```



#### Plan B

1. 굳이 내림차순으로 정렬할 필요가 없는 것 같다.
2. 굳이 ArrayList를 쓰지 않고(for문으로 탐색, remove 하는 시간이 오래걸릴 것 같다), 스택이나 큐를 사용해보면 어떨까? 스택, 큐를 연습해 볼 수 있는 좋은 기회일 수도 있다.