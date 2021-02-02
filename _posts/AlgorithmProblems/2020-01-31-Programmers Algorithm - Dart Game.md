---
layout: post
title: 프로그래머스 다트게임 - 2018 카카오 블라인드
subtitle: Algorithm Solution
background: '/img/bg_technology.jpg'
categories: technology
---

```
카카오톡 게임별의 하반기 신규 서비스로 다트 게임을 출시하기로 했다. 다트 게임은 다트판에 다트를 세 차례 던져 그 점수의 합계로 실력을 겨루는 게임으로, 모두가 간단히 즐길 수 있다.
갓 입사한 무지는 코딩 실력을 인정받아 게임의 핵심 부분인 점수 계산 로직을 맡게 되었다. 다트 게임의 점수 계산 로직은 아래와 같다.

다트 게임은 총 3번의 기회로 구성된다.
각 기회마다 얻을 수 있는 점수는 0점에서 10점까지이다.
점수와 함께 Single(S), Double(D), Triple(T) 영역이 존재하고 각 영역 당첨 시 점수에서 1제곱, 2제곱, 3제곱 (점수1 , 점수2 , 점수3 )으로 계산된다.
옵션으로 스타상(*) , 아차상(#)이 존재하며 스타상(*) 당첨 시 해당 점수와 바로 전에 얻은 점수를 각 2배로 만든다. 아차상(#) 당첨 시 해당 점수는 마이너스된다.
스타상(*)은 첫 번째 기회에서도 나올 수 있다. 이 경우 첫 번째 스타상(*)의 점수만 2배가 된다. (예제 4번 참고)
스타상(*)의 효과는 다른 스타상(*)의 효과와 중첩될 수 있다. 이 경우 중첩된 스타상(*) 점수는 4배가 된다. (예제 4번 참고)
스타상(*)의 효과는 아차상(#)의 효과와 중첩될 수 있다. 이 경우 중첩된 아차상(#)의 점수는 -2배가 된다. (예제 5번 참고)
Single(S), Double(D), Triple(T)은 점수마다 하나씩 존재한다.
스타상(*), 아차상(#)은 점수마다 둘 중 하나만 존재할 수 있으며, 존재하지 않을 수도 있다.
0~10의 정수와 문자 S, D, T, *, #로 구성된 문자열이 입력될 시 총점수를 반환하는 함수를 작성하라.

입력 형식
점수|보너스|[옵션]으로 이루어진 문자열 3세트.
예) 1S2D*3T

점수는 0에서 10 사이의 정수이다.
보너스는 S, D, T 중 하나이다.
옵선은 *이나 # 중 하나이며, 없을 수도 있다.
출력 형식
3번의 기회에서 얻은 점수 합계에 해당하는 정수값을 출력한다.
예) 37

입출력 예제
예제	dartResult	answer	설명
1	1S2D*3T	37	11 * 2 + 22 * 2 + 33
2	1D2S#10S	9	12 + 21 * (-1) + 101
3	1D2S0T	3	12 + 21 + 03
4	1S*2T*3S	23	11 * 2 * 2 + 23 * 2 + 31
5	1D#2S*3S	5	12 * (-1) * 2 + 21 * 2 + 31
6	1T2D3D#	-4	13 + 22 + 32 * (-1)
7	1D2S3T*	59	12 + 21 * 2 + 33 * 2

출처 : https://programmers.co.kr/learn/courses/30/lessons/17682
```



---

#### My solution (Java)

```java
class Solution {
	int[] numList = new int[3];
	String[] sdtList = new String[3];
	String[] specialList = new String[3];
	int[] calcul = new int[3];

	public int solution(String dartResult) {
		int answer = 0;
		String[] list = dartResult.split("");
		split(list, 0);

		/* 각 단어 묶음 SDT 계산 */
		SDTcalculation(0);
		SDTcalculation(1);
		SDTcalculation(2);

		/* #, * 옵션 계산 */
		finalCalcul(0);
		finalCalcul(1);
		finalCalcul(2);
		
		answer += calcul[0] + calcul[1] + calcul[2];
		return answer;
	}

	/* target String을 3부분으로 자르고 각각의 list에 담는 메소드 (재귀 사용) */
	public void split(String[] list, int countCheck) {
		if (countCheck > 2) {
			return;
		}
		int count = 0;
		String[] newList;
		for (int i = 0; i < list.length; i++) {
			count++;
			if (list[i].equals("S") || list[i].equals("D") || list[i].equals("T")) {
				sdtList[countCheck] = list[i];
				String result = "";
				for (int j = 0; j < count - 1; j++) {
					result += list[j];
				}
				numList[countCheck] = Integer.parseInt(result);

				if ((i + 1) == list.length) {
				} else if (list[i + 1].equals("#") || list[i + 1].equals("*")) {
					specialList[countCheck] = list[i + 1];
					count++;
				}

				newList = new String[list.length - count];
				int num = 0;
				for (int k = count; k < list.length; k++) {
					newList[num] = list[k];
					num++;
				}
				split(newList, countCheck + 1);
				break;
			}
		}
	}

	/* SDT 계산해서 calcul[]에 담는 메소드 */
	public void SDTcalculation(int target) {
		int result = 0;
		if (sdtList[target].equals("S")) {
			result = numList[target];
			calcul[target] = result;
		} else if (sdtList[target].equals("D")) {
			result = numList[target] * numList[target];
			calcul[target] = result;
		} else {
			result = numList[target] * numList[target] * numList[target];
			calcul[target] = result;
		}
	}

	public void finalCalcul(int target) {
		if (specialList[target] == null) {
		} else if (specialList[target].equals("*")) {
			calcul[target] = calcul[target] * 2;
			if (target > 0) {
				calcul[target - 1] = calcul[target - 1] * 2;
			}
		} else if (specialList[target].equals("#")) {
			calcul[target] = -calcul[target];
		}
	}
}
```



---

#### Feedback

```
반복문으로만 풀 수 있는 쉬운 문제이나, 반복문 대신 재귀를 시도해봤다.
또한 빡빡한 코딩 보다는 메소드를 생성해서 코드의 수를 최대한 줄이려고 시도했는데,
가독성도 좋고 괜찮은 것 같다.
```

