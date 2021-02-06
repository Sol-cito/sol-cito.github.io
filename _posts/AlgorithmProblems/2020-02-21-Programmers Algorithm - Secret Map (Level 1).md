---
layout: post
title: 프로그래머스 비밀지도[1차] - 2018 카카오 블라인드 (Level 1)
subtitle: Programmers Algorithm Test Solution
background: '/img/bg_technology.jpg'
categories: technology/tech-algorithm
---

##### 문제 설명

비밀지도
네오는 평소 프로도가 비상금을 숨겨놓는 장소를 알려줄 비밀지도를 손에 넣었다. 그런데 이 비밀지도는 숫자로 암호화되어 있어 위치를 확인하기 위해서는 암호를 해독해야 한다. 다행히 지도 암호를 해독할 방법을 적어놓은 메모도 함께 발견했다.

지도는 한 변의 길이가 n인 정사각형 배열 형태로, 각 칸은 공백(" ) 또는벽(#") 두 종류로 이루어져 있다.
전체 지도는 두 장의 지도를 겹쳐서 얻을 수 있다. 각각 지도 1과 지도 2라고 하자. 지도 1 또는 지도 2 중 어느 하나라도 벽인 부분은 전체 지도에서도 벽이다. 지도 1과 지도 2에서 모두 공백인 부분은 전체 지도에서도 공백이다.
지도 1과 지도 2는 각각 정수 배열로 암호화되어 있다.
암호화된 배열은 지도의 각 가로줄에서 벽 부분을 1, 공백 부분을 0으로 부호화했을 때 얻어지는 이진수에 해당하는 값의 배열이다.

네오가 프로도의 비상금을 손에 넣을 수 있도록, 비밀지도의 암호를 해독하는 작업을 도와줄 프로그램을 작성하라.

##### 입력 형식
입력으로 지도의 한 변 크기 n 과 2개의 정수 배열 arr1, arr2가 들어온다.

1 ≦ n ≦ 16
arr1, arr2는 길이 n인 정수 배열로 주어진다.
정수 배열의 각 원소 x를 이진수로 변환했을 때의 길이는 n 이하이다. 즉, 0 ≦ x ≦ 2n - 1을 만족한다.
출력 형식
원래의 비밀지도를 해독하여 '#', 공백으로 구성된 문자열 배열로 출력하라.

##### 입출력 예제
매개변수	값
n	5
arr1	[9, 20, 28, 18, 11]
arr2	[30, 1, 21, 17, 28]
출력	["#####","# # #", "### #", "# ##", "#####"]

매개변수	값
n	6
arr1	[46, 33, 33 ,22, 31, 50]
arr2	[27 ,56, 19, 14, 14, 10]
출력	["######", "### #", "## ##", " #### ", " #####", "### # "]


출처 : https://programmers.co.kr/learn/courses/30/lessons/17681

---

#### My solution (Java)

```
class Solution {
	public String[] solution(int n, int[] arr1, int[] arr2) {
		String[] answer = {};
		String[] arr1_binary = new String[n];
		String[] arr2_binary = new String[n];

		for (int i = 0; i < n; i++) {
			arr1_binary[i] = TenToTwo(arr1[i]);
			String[] x = arr1_binary[i].split("");
			if (arr1_binary[i].length() < n) {
				String redefine = "";
				for (int j = 0; j < n - arr1_binary[i].length(); j++) {
					redefine += "0";
				}
				int count = 0;
				for (int j = n - arr1_binary[i].length(); j < n; j++) {
					redefine += x[count];
					count++;
				}
				arr1_binary[i] = redefine;
			} else {
				arr1_binary[i] = TenToTwo(arr1[i]);
			}
		}
		for (int i = 0; i < n; i++) {
			arr2_binary[i] = TenToTwo(arr2[i]);
			String[] x = arr2_binary[i].split("");
			if (arr2_binary[i].length() < n) {
				String redefine = "";
				for (int j = 0; j < n - arr2_binary[i].length(); j++) {
					redefine += "0";
				}
				int count = 0;
				for (int j = n - arr2_binary[i].length(); j < n; j++) {
					redefine += x[count];
					count++;
				}
				arr2_binary[i] = redefine;
			} else {
				arr2_binary[i] = TenToTwo(arr2[i]);
			}
		}
		answer = new String[n];
		for (int i = 0; i < n; i++) {
			String put = "";
			String[] apart_arr1 = new String[n];
			apart_arr1 = arr1_binary[i].split("");
			String[] apart_arr2 = new String[n];
			apart_arr2 = arr2_binary[i].split("");
			for (int j = 0; j < n; j++) {
				if (apart_arr1[j].equals("1") || apart_arr2[j].equals("1")) {
					put += "#";
				} else {
					put += " ";
				}
			}
			answer[i] = put;
			System.out.println("answer[" + i + "] : " + put);
		}
		return answer;
	}

	public static String TenToTwo(int decimal) { // 10진수 -> 2진수 변환 메소드
		int div = 1;
		int digit = 0;
		int[] binary = {};
		String answer = "";

		while (true) {
			if (decimal / div >= 1) {
				div = div * 2;
				digit++;
			} else {
				if (decimal == 0) {
					return "0";
				}
				div = div / 2;
				binary = new int[digit];
				while (true) {
					binary[digit - 1] = decimal / div;
					if (decimal % div == 0) {
						break;
					} else {
						decimal = decimal - div * (decimal / div);
						div = div / 2;
						digit--;
					}
				}
				break;
			}
		}
		for (int i = binary.length - 1; i >= 0; i--) {
			answer += binary[i];
		}
		return answer;
	}
}
```
---


#### Feedback

```
코드가 굉장히 길다. 그 이유는 
1) 10진수 -> 2진수 변환 메소드를 직접 구현하였기 때문
2) 2진수 연산 OR 를 쓰지 않고 if~else 로 풀었기 때문

테스트 케이스 에러가 났었는데, 0이 input으로 들어왔을 때의 2진수변환 메소드를 따로 처리하지 않아 에러가 났었다. 앞으로 신경써야 할듯..

스터디에서 나눌 내용은
1) 진수 변환에 대한 Java 내장 함수가 있는가?
2) 2진수 연산 Or란 어떻게 구현하는가?
```