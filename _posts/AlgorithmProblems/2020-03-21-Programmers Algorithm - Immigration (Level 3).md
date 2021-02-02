---
layout: post
title: 프로그래머스 이분탐색 - 입국심사 (Level 3)
subtitle: Algorithm Solution
background: '/img/bg_technology.jpg'
categories: technology
---

**문제 설명**

n명이 입국심사를 위해 줄을 서서 기다리고 있습니다. 각 입국심사대에 있는 심사관마다 심사하는데 걸리는 시간은 다릅니다.

처음에 모든 심사대는 비어있습니다. 한 심사대에서는 동시에 한 명만 심사를 할 수 있습니다. 가장 앞에 서 있는 사람은 비어 있는 심사대로 가서 심사를 받을 수 있습니다. 하지만 더 빨리 끝나는 심사대가 있으면 기다렸다가 그곳으로 가서 심사를 받을 수도 있습니다.

모든 사람이 심사를 받는데 걸리는 시간을 최소로 하고 싶습니다.

입국심사를 기다리는 사람 수 n, 각 심사관이 한 명을 심사하는데 걸리는 시간이 담긴 배열 times가 매개변수로 주어질 때, 모든 사람이 심사를 받는데 걸리는 시간의 최솟값을 return 하도록 solution 함수를 작성해주세요.


**제한사항**

입국심사를 기다리는 사람은 1명 이상 1,000,000,000명 이하입니다.
각 심사관이 한 명을 심사하는데 걸리는 시간은 1분 이상 1,000,000,000분 이하입니다.
심사관은 1명 이상 100,000명 이하입니다.

**입출력 예**

n	times	return
6	[7, 10]	28


**입출력 예 설명**

가장 첫 두 사람은 바로 심사를 받으러 갑니다.

7분이 되었을 때, 첫 번째 심사대가 비고 3번째 사람이 심사를 받습니다.

10분이 되었을 때, 두 번째 심사대가 비고 4번째 사람이 심사를 받습니다.

14분이 되었을 때, 첫 번째 심사대가 비고 5번째 사람이 심사를 받습니다.

20분이 되었을 때, 두 번째 심사대가 비지만 6번째 사람이 그곳에서 심사를 받지 않고 1분을 더 기다린 후에 첫 번째 심사대에서 심사를 받으면 28분에 모든 사람의 심사가 끝납니다.



출처 : https://programmers.co.kr/learn/courses/30/lessons/43238


---

#### My first solution (Java) - Fail (exceeds time limit)

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.LinkedList;
import java.util.Queue;
class Solution {
	public long solution(int n, int[] times) {
		long answer = 0;

		/* 대기인을 queue에 담음 */
		Queue<Integer> people = new LinkedList<Integer>();
		for (int i = 0; i < n; i++) {
			people.add(i);
		}

		/* 심사관들이 담긴 list */
		ArrayList<Officer> officerList = new ArrayList<Officer>();
		/* 각 심사관의 대기시간이 담긴 Map */
		HashMap<Integer, Integer> originalWaitingTime = new HashMap<Integer, Integer>();

		/* Immigration Officer 생성 + list와 2개의 map에 담음 */
		for (int i = 0; i < times.length; i++) {
			Officer officer = new Officer(i, times[i]);
			officerList.add(officer);
			originalWaitingTime.put(i, times[i]);
		}

		while (true) {
			if (people.isEmpty()) {
				break;
			}
			/* 현재 가장 짧은 대기시간을 구함(original 대기시간 말고 '현재'대기시간) */
			int shortest = officerList.get(0).time;
			for (int i = 1; i < officerList.size(); i++) {
				shortest = Math.min(shortest, officerList.get(i).time);
			}
			/* 가장 짧은 시간의 대기시간에 해당되는 officers의 ID를 Hash에 담는다 */
			HashMap<Integer, Boolean> shortOfficerMap = new HashMap<Integer, Boolean>();
			for (int i = 0; i < officerList.size(); i++) {
				if (officerList.get(i).time == shortest) {
					shortOfficerMap.put(i, true);
				}
			}
			/*
			 * 가장 짧은 시간의 대기시간에 해당되는 officers은 다시 시간을 원래대로 초기화하고, 나머지 모든 심사관들은 가장 짧은시간만큼을 뺀다.
			 */
			for (int i = 0; i < officerList.size(); i++) {
				if (shortOfficerMap.get(i) != null) {
					int originalTime = originalWaitingTime.get(i);
					officerList.get(i).time = originalTime;
					continue;
				}
				officerList.get(i).time -= shortest;
			}
			/* 가장 짧은 시간의 대기시간에 해당되는 officers 수만큼 대기열에서 people을 poll */
			int size = shortOfficerMap.size();
			for (int i = 0; i < size; i++) {
				people.poll();
			}
			/* answer에 가장 짧은 대기시간을 더함 */
			answer += shortest;
		}
		return answer;
	}
}

class Officer {
	int id;
	int time;

	public Officer(int id, int time) {
		this.id = id;
		this.time = time;
	}
}
```

---

#### My first solution (Java) - Success

```
import java.util.Arrays;
class Solution {
	public long solution(int n, int[] times) {
		long answer = 0;
		Arrays.sort(times);
		/* 초기값 left, right 세팅 */
		long left = 0;
		long right = (long) (n * (long) times[times.length - 1]);
		long mid = (left + right) / 2;
		long totalCheckedPeople = 0;
		answer = mid;
		while (true) {
			if (mid == left) {
				break;
			}
			totalCheckedPeople = 0;
			for (int i = 0; i < times.length; i++) {
				totalCheckedPeople += mid / (long) times[i];
			}
			if (totalCheckedPeople < n) { 
			// 심사관들이 체크한 사람 수가 전체 사람 수보다 적으면 -> 시간을 늘린다.
				left = mid;
				mid = (left + right) / 2;
			} else { 
			// 심사관들이 체크한 사람 수가 전체 사람 수보다 많거나 같으면 -> 시간을 줄인다.
				answer = Math.min(answer, mid);
				right = mid;
				mid = (left + right) / 2;
			}
		}
		return answer;
	}
}
```



---

본 문제는, 처음에는 이분탐색이 아니라 심사관 클래스를 따로 만들고 객체를 선언하여 시간을 계속 더해가는 방식으로 풀려고 했다. 

접근 자체는 틀리지 않은 것 같으나, 10억명이라는 사람(...)과 최대 10억분이라는 심사 시간, 게다가 10만명의 심사관이 있으니, 객체를 만들어서 심사관 한명한명 시간을 빼려다보니 테스트케이스 3개 빼고는 전부 시간초과가 발생했다.

그래서, 문제 분류대로 이분탐색으로 문제풀이를 시도했고, 결국 테스트케이스를 전부 통과했다.



#### My logic & feedback

#####  1. 최초 로직 (Fail)

1) 대기인들을 전부 Queue에 담는다

2) 심사관 객체를 생성하는데, 심사관은 심사 시간 및 고유의 id를 멤버변수로 가진다. 

3) 심사관 객체를 탐색하면서, 가장 짧은 심사시간을 가진 객체의 id를 Hash에 저장한다 ( Time complexity = **O(n)** )

4) 가장 짧은 심사시간을 가진 심사관은 시간을 원래대로 초기화하고, 나머지 심사관들은 가장 짧은 시간만큼 빼준다. 가장 짧은 심사시간을 가진 심사관만큼 Queue를 poll한다.

5) 가장 짧은 시간만큼을 answer에 반복적으로 더한다.

6) 이 과정을 계속 반복하다가, Queue is empty 가 되면 종료한다.

 => Worst Case는, 대기인이 10억명, 심사관이 10만명 있는데, 심사관들 중 가장 짧은 심사시간이 1분이고, 나머지는 10억분인 경우이다. 10만개의 심사관 객체를 탐색하는 시간복잡도는 O(n) 이고, while반복을 10억번 해야만 한다(...). 당연히 시간초과가 날 수밖에 없다.

##### 2. 이분탐색 로직 (Success)

1) 심사에 필요한 최소 시간 left, 최대 시간 right 를 선언한다.

2) left와 right의 중앙값 mid를 선언한다.

3) while문을 도는데, for문으로 각 심사관들이 주어진 시간(mid)동안 다 합쳐서 몇 명을 심사할 수 있는지(totalChekcedPeople)를 계산한 후, left를 늘릴지, right를 줄일지 결정한다.

4) 이렇게 이분탐색을 계속하다가 mid가 left와 같아지는 경우에 닿게 되는데, 이 지점이 mid가 움직이지 않는 마지막 지점이다.

5) 따라서 mid=left가 되는 시점에 닿았을 때까지,  totalCheckedPeople < n를 만족하는 mid의 최소값이 정답이다.