---
layout: post
title: 프로그래머스 괄호변환 - 2020 Kakao blind (Level 2)
subtitle: Algorithm Solution
background: '/img/bg_technology.jpg'
categories: technology
---


### 용어의 정의

**'('** 와 **')'** 로만 이루어진 문자열이 있을 경우, '(' 의 개수와 ')' 의 개수가 같다면 이를 **`균형잡힌 괄호 문자열`**이라고 부릅니다.
그리고 여기에 '('와 ')'의 괄호의 짝도 모두 맞을 경우에는 이를 **`올바른 괄호 문자열`**이라고 부릅니다.
예를 들어, `"(()))("`와 같은 문자열은 균형잡힌 괄호 문자열 이지만 올바른 괄호 문자열은 아닙니다.
반면에 `"(())()"`와 같은 문자열은 균형잡힌 괄호 문자열 이면서 동시에 올바른 괄호 문자열 입니다.

'(' 와 ')' 로만 이루어진 문자열 w가 균형잡힌 괄호 문자열 이라면 다음과 같은 과정을 통해 올바른 괄호 문자열로 변환할 수 있습니다.

```
1. 입력이 빈 문자열인 경우, 빈 문자열을 반환합니다. 
2. 문자열 w를 두 "균형잡힌 괄호 문자열" u, v로 분리합니다. 단, u는 "균형잡힌 괄호 문자열"로 더 이상 분리할 수 없어야 하며, v는 빈 문자열이 될 수 있습니다. 
3. 문자열 u가 "올바른 괄호 문자열" 이라면 문자열 v에 대해 1단계부터 다시 수행합니다. 
  3-1. 수행한 결과 문자열을 u에 이어 붙인 후 반환합니다. 
4. 문자열 u가 "올바른 괄호 문자열"이 아니라면 아래 과정을 수행합니다. 
  4-1. 빈 문자열에 첫 번째 문자로 '('를 붙입니다. 
  4-2. 문자열 v에 대해 1단계부터 재귀적으로 수행한 결과 문자열을 이어 붙입니다. 
  4-3. ')'를 다시 붙입니다. 
  4-4. u의 첫 번째와 마지막 문자를 제거하고, 나머지 문자열의 괄호 방향을 뒤집어서 뒤에 붙입니다. 
  4-5. 생성된 문자열을 반환합니다.
```

**균형잡힌 괄호 문자열** p가 매개변수로 주어질 때, 주어진 알고리즘을 수행해 **올바른 괄호 문자열**로 변환한 결과를 return 하도록 solution 함수를 완성해 주세요.

### 매개변수 설명

- p는 '(' 와 ')' 로만 이루어진 문자열이며 길이는 2 이상 1,000 이하인 짝수입니다.
- 문자열 p를 이루는 '(' 와 ')' 의 개수는 항상 같습니다.
- 만약 p가 이미 올바른 괄호 문자열이라면 그대로 return 하면 됩니다.

------

### 입출력 예

| p            | result       |
| ------------ | ------------ |
| `"(()())()"` | `"(()())()"` |
| `")("`       | `"()"`       |
| `"()))((()"` | `"()(())()"` |

### 입출력 예에 대한 설명

**입출력 예 #1**
이미 올바른 괄호 문자열 입니다.

**입출력 예 #2**

- 두 문자열 u, v로 분리합니다.

  - u = `")("`
  - v = `""`

- u가 올바른 괄호 문자열

  이 아니므로 다음과 같이 새로운 문자열을 만듭니다.

  - v에 대해 1단계부터 재귀적으로 수행하면 빈 문자열이 반환됩니다.
  - u의 앞뒤 문자를 제거하고, 나머지 문자의 괄호 방향을 뒤집으면 `""`이 됩니다.
  - 따라서 생성되는 문자열은 `"("` + `""` + `")"` + `""`이며, 최종적으로 `"()"`로 변환됩니다.

**입출력 예 #3**

- 두 문자열 u, v로 분리합니다.

  - u = `"()"`
  - v = `"))((()"`

- 문자열 u가 올바른 괄호 문자열이므로 그대로 두고, v에 대해 재귀적으로 수행합니다.

- 다시 두 문자열 u, v로 분리합니다.

  - u = `"))(("`
  - v = `"()"`

- u가

   올바른 괄호 문자열이 아니므로 다음과 같이 새로운 문자열을 만듭니다.

  - v에 대해 1단계부터 재귀적으로 수행하면 `"()"`이 반환됩니다.
  - u의 앞뒤 문자를 제거하고, 나머지 문자의 괄호 방향을 뒤집으면 `"()"`이 됩니다.
  - 따라서 생성되는 문자열은 `"("` + `"()"` + `")"` + `"()"`이며, 최종적으로 `"(())()"`를 반환합니다.

- 처음에 그대로 둔 문자열에 반환된 문자열을 이어 붙이면 `"()"` + `"(())()"` = `"()(())()"`가 됩니다.



출처 : https://programmers.co.kr/learn/courses/30/lessons/60058



---

#### My first solution (Java) 

```java
import java.util.ArrayList;

class Solution {
	public String solution(String p) {
		String answer = "";

		char[] list = p.toCharArray();

		/* 0이면 빈 문자열 반환 */
		if (list.length == 0) {
			return "";
		}

		String[] uv = new String[2];
		uv = getUandV(p);

		/* 재귀를 돌아서 답을 도출 */
		answer = "" + recursion(uv[0], uv[1]);
		return answer;
	}
	
	/* 재귀 메소드 */
	public String recursion(String u, String v) {
		/* v가 공백일 경우 : 조건에 맞게 u만 조작해서 return한다 */
		if (v.length() == 0) {
			if (whetherRightOrNot(u) == true) {
				return u;
			}
			StringBuilder sb = new StringBuilder();
			sb.append("()");
			char[] uList = u.toCharArray();
			for (int i = 1; i < uList.length - 1; i++) {
				if (uList[i] == '(') {
					sb.append(")");
				} else {
					sb.append("(");
				}
			}
			return sb.toString();
		}
		if (whetherRightOrNot(u) == true) {
			StringBuilder sb = new StringBuilder();
			String[] uv = new String[2];
			uv = getUandV(v);
			String result_v = recursion(uv[0], uv[1]);
			sb.append(u);
			sb.append(result_v);
			return sb.toString();
		}
		StringBuilder sb1 = new StringBuilder();
		sb1.append("(");
		String[] uv = new String[2];
		uv = getUandV(v);
		String result_v = recursion(uv[0], uv[1]);
		sb1.append(result_v);
		sb1.append(")");
		StringBuilder sb2 = new StringBuilder();
		char[] uList = u.toCharArray();
		for (int i = 1; i < uList.length - 1; i++) {
			if (uList[i] == '(') {
				sb2.append(")");
			} else {
				sb2.append("(");
			}
		}
		sb1.append(sb2);
		return sb1.toString();
	}
	
	/* 주어진 String이 "올바른 괄호 문자열"인지 아닌지 검증하는 메소드 */
	public boolean whetherRightOrNot(String target) {
		ArrayList<Character> list = new ArrayList<Character>();
		int size = target.length();
		for (int i = 0; i < target.toCharArray().length; i++) {
			list.add(target.toCharArray()[i]);
		}
		int pointer = 1;
		int count = 0;
		while (true) {
			if (target.charAt(0) == ')') {
				return false;
			}
			if (pointer >= list.size()) {
				break;
			}
			if (list.get(pointer) == ')') {
				list.remove(pointer);
				list.remove(0);
				count += 2;
				pointer = 1;
			} else {
				pointer++;
			}
		}
		if (count == size) {
			return true;
		}
		return false;
	}

	/* 주어진 String을 U와 V로 분리해내는 메소드 */
	public String[] getUandV(String target) {
		char[] list = target.toCharArray();
		char first = list[0];
		int numberOfFirst = 1;
		int numberOfSecond = 0;
		int pointer = 1;
		while (true) {
			if (numberOfFirst == numberOfSecond) {
				break;
			}
			if (list[pointer] == first) {
				numberOfFirst++;
			} else {
				numberOfSecond++;
			}
			pointer++;
		}
		String[] result = new String[2];
		StringBuilder sb1 = new StringBuilder();
		for (int i = 0; i < pointer; i++) {
			sb1.append(list[i]);
		}
		result[0] = sb1.toString();
		StringBuilder sb2 = new StringBuilder();
		for (int i = pointer; i < list.length; i++) {
			sb2.append(list[i]);
		}
		result[1] = sb2.toString();
		return result;
	}
}
```

---

#### My logic & feedback

Level 2 문제라서 방심했는데..생각보다 어려운 문제였다.

문제의 풀이는, 문제에서 주어진 알고리즘을 재귀로 충실히 구현하면 되나...구현이 생각보다 어려웠다.

즉, [ 주어진 String이 "올바른 괄호 문자열"인지 아닌지 검증하는 메소드]와 [주어진 String을 U와 V로 분리해내는 메소드] 를 구현하고, 재귀를 계속 돌면서 답을 도출하면 된다.

위 두 메소드 모두 int pointer 를 선언하고 파라미터로 들어온 String을 char[]로 변환한 후, char[] 의 각 index를 가리키면서 로직을 수행하는 것으로 구현하였다.

whetherRightOrNot 메소드는 ArrayList를 구현하여 해당되는 char를 remove하는 형태로 구현하였다.