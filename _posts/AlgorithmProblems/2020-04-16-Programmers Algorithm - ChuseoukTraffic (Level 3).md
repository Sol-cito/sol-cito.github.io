---
layout: post
title: "프로그래머스 카카오 - 추석트래픽 (Level 3)"
subtitle: Algorithm Solution
background: '/img/bg_technology.jpg'
categories: technology-algorithm

---

###### 문제 설명

이번 추석에도 시스템 장애가 없는 명절을 보내고 싶은 어피치는 서버를 증설해야 할지 고민이다. 장애 대비용 서버 증설 여부를 결정하기 위해 작년 추석 기간인 9월 15일 로그 데이터를 분석한 후 초당 최대 처리량을 계산해보기로 했다. **초당 최대 처리량**은 요청의 응답 완료 여부에 관계없이 임의 시간부터 1초(=1,000밀리초)간 처리하는 요청의 최대 개수를 의미한다.

### 입력 형식

- `solution` 함수에 전달되는 `lines` 배열은 **N**(1 ≦ **N** ≦ 2,000)개의 로그 문자열로 되어 있으며, 각 로그 문자열마다 요청에 대한 응답완료시간 **S**와 처리시간 **T**가 공백으로 구분되어 있다.
- 응답완료시간 **S**는 작년 추석인 2016년 9월 15일만 포함하여 고정 길이 `2016-09-15 hh:mm:ss.sss` 형식으로 되어 있다.
- 처리시간 **T**는 `0.1s`, `0.312s`, `2s` 와 같이 최대 소수점 셋째 자리까지 기록하며 뒤에는 초 단위를 의미하는 `s`로 끝난다.
- 예를 들어, 로그 문자열 `2016-09-15 03:10:33.020 0.011s`은 2016년 9월 15일 오전 3시 10분 **33.010초**부터 2016년 9월 15일 오전 3시 10분 **33.020초**까지 **0.011초** 동안 처리된 요청을 의미한다. **(처리시간은 시작시간과 끝시간을 포함)**
- 서버에는 타임아웃이 3초로 적용되어 있기 때문에 처리시간은 **0.001 ≦ T ≦ 3.000**이다.
- `lines` 배열은 응답완료시간 **S**를 기준으로 오름차순 정렬되어 있다.

### 출력 형식

- `solution` 함수에서는 로그 데이터 `lines` 배열에 대해 **초당 최대 처리량**을 리턴한다.

### 입출력 예제

#### 예제1

- 입력: [
  2016-09-15 01:00:04.001 2.0s,
  2016-09-15 01:00:07.000 2s
  ]
- 출력: 1

#### 예제2

- 입력: [
  2016-09-15 01:00:04.002 2.0s,
  2016-09-15 01:00:07.000 2s
  ]
- 출력: 2
- 설명: 처리시간은 시작시간과 끝시간을 **포함**하므로
  첫 번째 로그는 `01:00:02.003 ~ 01:00:04.002`에서 2초 동안 처리되었으며,
  두 번째 로그는 `01:00:05.001 ~ 01:00:07.000`에서 2초 동안 처리된다.
  따라서, 첫 번째 로그가 끝나는 시점과 두 번째 로그가 시작하는 시점의 구간인 `01:00:04.002 ~ 01:00:05.001` 1초 동안 최대 2개가 된다.

#### 예제3

- 입력: [
  2016-09-15 20:59:57.421 0.351s,
  2016-09-15 20:59:58.233 1.181s,
  2016-09-15 20:59:58.299 0.8s,
  2016-09-15 20:59:58.688 1.041s,
  2016-09-15 20:59:59.591 1.412s,
  2016-09-15 21:00:00.464 1.466s,
  2016-09-15 21:00:00.741 1.581s,
  2016-09-15 21:00:00.748 2.31s,
  2016-09-15 21:00:00.966 0.381s,
  2016-09-15 21:00:02.066 2.62s
  ]
- 출력: 7
- 설명: 아래 타임라인 그림에서 빨간색으로 표시된 1초 각 구간의 처리량을 구해보면 `(1)`은 4개, `(2)`는 7개, `(3)`는 2개임을 알 수 있다. 따라서 **초당 최대 처리량**은 7이 되며, 동일한 최대 처리량을 갖는 1초 구간은 여러 개 존재할 수 있으므로 이 문제에서는 구간이 아닌 개수만 출력한다.
  ![Timeline](http://t1.kakaocdn.net/welcome2018/chuseok-01-v5.png)

출처 : https://programmers.co.kr/learn/courses/30/lessons/17676



---

#### My first solution (Java)

```java
import java.util.ArrayList;

class Traffic {
    public int solution(String[] lines) {
        ArrayList<Log> logList = new ArrayList<>();
        int answer = 0;
        for (int i = 0; i < lines.length; i++) {
            String[] chunks = lines[i].split(" "); 
            // 0 - 년월일, 1 - 시:분:초.밀리초 , 2 - 소요시간
            char[] list = chunks[1].toCharArray();
            String hh = "" + list[0] + list[1];
            String mm = "" + list[3] + list[4];
            String ss = "" + list[6] + list[7];
            String milss = "" + list[9] + list[10] + list[11];
            int finishTime = Integer.parseInt(milss) + Integer.parseInt(ss) * 1000 + Integer.parseInt(mm) * 1000 * 60 +
                    Integer.parseInt(hh) * 1000 * 3600;
            String nextOne = chunks[2].split("s")[0];
            char[] nextList = nextOne.toCharArray();
            int result = 0;
            int multi = 1000;
            for (int j = 0; j < nextList.length; j++) {
                if (nextList[j] != '.') {
                    int target = Integer.parseInt("" + nextList[j]);
                    result += target * multi;
                    multi /= 10;
                }
            }
            int startTime = finishTime - result + 1;
            Log log = new Log(startTime, finishTime);
            logList.add(log);
        }
        /* 각 log를 돌면서 탐색할 시간대(startTime, finishTime)를 list에 담음 */
        ArrayList<Integer> logTimeList = new ArrayList<>();
        for (int i = 0; i < logList.size(); i++) {
            Log log = logList.get(i);
            logTimeList.add(log.startTime);
            logTimeList.add(log.finishTime);
        }

        for (int i = 0; i < logTimeList.size(); i++) {
            int count = 0;
            int eachStarTime = logTimeList.get(i);
            int eachFinishTime = logTimeList.get(i) + 999;
            for (int j = 0; j < logList.size(); j++) {
                Log log = logList.get(j);
                if (log.startTime <= eachStarTime && log.finishTime >= eachFinishTime) {
                    count++;
                } else {
                    if (log.startTime >= eachStarTime && log.startTime <= eachFinishTime) {
                        count++;
                        continue;
                    }
                    if (log.finishTime >= eachStarTime && log.finishTime <= eachFinishTime) {
                        count++;
                    }
                }
            }
            answer = Math.max(answer, count);
        }
        return answer;
    }
}

class Log {
    int startTime;
    int finishTime;

    public Log(int startTime, int finishTime) {
        this.startTime = startTime;
        this.finishTime = finishTime;
    }

```

---

#### My logic & feedback

이 문제 해결의 핵심 포인트는 두 가지다.

1) 주어진 Log를 어떻게 parsing할것인가

2) 초당 최대 처리량을 어떻게 구할 것인가

파싱의 경우 split과 toCharArray를 이용하였는데, 코드가 좀 지저분해졌다. 더 좋은 방법이 분명 있을 것 같다.

2)번 문제에서...첫 시도에서는 내가 만든 Log 객체들의 startTime과 finishTime 사이의 모든 시간대를 초 단위로 검사하는 방법을 썼는데, 시간초과가 났다(당연).

팀원들과 스터디 이후, 아이디어를 얻어, Log 객체들의 StartTime, FinishTime만 ArrayList에 넣어 검사했더니 통과하였다.