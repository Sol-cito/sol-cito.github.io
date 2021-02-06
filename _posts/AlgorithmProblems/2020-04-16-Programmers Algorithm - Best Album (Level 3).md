---
layout: post
title: "프로그래머스 해시 - 베스트앨범 (Level 3)"
subtitle: Algorithm Solution
background: '/img/bg_technology.jpg'
categories: technology/tech-algorithm
---


**문제 설명**

스트리밍 사이트에서 장르 별로 가장 많이 재생된 노래를 두 개씩 모아 베스트 앨범을 출시하려 합니다. 노래는 고유 번호로 구분하며, 노래를 수록하는 기준은 다음과 같습니다.

1. 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.

노래의 장르를 나타내는 문자열 배열 genres와 노래별 재생 횟수를 나타내는 정수 배열 plays가 주어질 때, 베스트 앨범에 들어갈 노래의 고유 번호를 순서대로 return 하도록 solution 함수를 완성하세요.

##### 제한사항

- genres[i]는 고유번호가 i인 노래의 장르입니다.
- plays[i]는 고유번호가 i인 노래가 재생된 횟수입니다.
- genres와 plays의 길이는 같으며, 이는 1 이상 10,000 이하입니다.
- 장르 종류는 100개 미만입니다.
- 장르에 속한 곡이 하나라면, 하나의 곡만 선택합니다.
- 모든 장르는 재생된 횟수가 다릅니다.

##### 입출력 예

| genres                                | plays                      | return       |
| ------------------------------------- | -------------------------- | ------------ |
| [classic, pop, classic, classic, pop] | [500, 600, 150, 800, 2500] | [4, 1, 3, 0] |

##### 입출력 예 설명

classic 장르는 1,450회 재생되었으며, classic 노래는 다음과 같습니다.

- 고유 번호 3: 800회 재생
- 고유 번호 0: 500회 재생
- 고유 번호 2: 150회 재생

pop 장르는 3,100회 재생되었으며, pop 노래는 다음과 같습니다.

- 고유 번호 4: 2,500회 재생
- 고유 번호 1: 600회 재생

따라서 pop 장르의 [4, 1]번 노래를 먼저, classic 장르의 [3, 0]번 노래를 그다음에 수록합니다.

※ 공지 - 2019년 2월 28일 테스트케이스가 추가되었습니다.





출처 : https://programmers.co.kr/learn/courses/30/lessons/42579#



---

#### My first solution (Java)

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;

class Solution {
    public int[] solution(String[] genres, int[] plays) {
        int[] answer = {};
        HashMap<String, Integer> genreMap = new HashMap<>(); // key : 장르명 / value : 총 재생횟수
        HashMap<String, ArrayList<Song>> songMap = new HashMap<>(); // key : 고유키 / value : Song객체 arrayList
        ArrayList<String> genreList = new ArrayList<>(); // genre명 담긴 list
        HashMap<Integer, Song> eachSongMap = new HashMap<>();
        for (int i = 0; i < genres.length; i++) {
            Song song = new Song(i, plays[i]);
            eachSongMap.put(i, song);
            ArrayList<Song> list = new ArrayList<>();
            if (songMap.get(genres[i]) == null) {
                songMap.put(genres[i], list);
                songMap.get(genres[i]).add(song);
            } else {
                songMap.get(genres[i]).add(song);
            }
            if (genreMap.get(genres[i]) == null) {
                genreList.add(genres[i]);
                genreMap.put(genres[i], plays[i]);
            } else {
                genreMap.replace(genres[i], genreMap.get(genres[i]) + plays[i]);
            }
        }
        int[] orderedList = new int[genreList.size()]; // 장르별 재생횟수 오름차순 정렬
        for (int i = 0; i < genreList.size(); i++) {
            orderedList[i] = genreMap.get(genreList.get(i));
        }
        Arrays.sort(orderedList);

        HashMap<Integer, String> reversedMap = new HashMap<>(); // key : 한 장르의 play 수 / value : 장르명
        for (int i = 0; i < genreList.size(); i++) {
            reversedMap.put(genreMap.get(genreList.get(i)), genreList.get(i));
        }
        ArrayList<Integer> finalResult = new ArrayList<>();
        int totalSize = 0;
        for (int i = orderedList.length - 1; i >= 0; i--) {
            ArrayList<Song> songList = songMap.get(reversedMap.get(orderedList[i]));
            int count = 0;
            int max = 0;
            if (songList.size() < 2) {
                max = 1;
            } else {
                max = 2;
            }
            int[] uniques = new int[2];
            uniques[1] = -1;
            int targetIndex = 0;
            boolean alreadyDelete = false;
            while (count < max) {
                int uniqueNumber = songList.get(0).uniqueNumber;
                int playTime = songList.get(0).playTime;
                for (int j = 1; j < songList.size(); j++) {
                    if (songList.get(j).playTime > playTime) {
                        playTime = songList.get(j).playTime;
                        uniqueNumber = songList.get(j).uniqueNumber;
                        targetIndex = j;
                    }
                }
                if (!alreadyDelete) {
                    songList.remove(targetIndex);
                    alreadyDelete = true;
                }
                uniques[count] = uniqueNumber;
                totalSize++;
                count++;
            }
            if (uniques[1] == -1) {
                finalResult.add(uniques[0]);
            } else {
                if (eachSongMap.get(uniques[0]) == eachSongMap.get(uniques[1])) {
                    if (uniques[0] < uniques[1]) {
                        finalResult.add(uniques[0]);
                        finalResult.add(uniques[1]);
                    } else {
                        finalResult.add(uniques[1]);
                        finalResult.add(uniques[0]);
                    }
                } else {
                    finalResult.add(uniques[0]);
                    finalResult.add(uniques[1]);
                }
            }
        }
        int index = 0;
        answer = new int[finalResult.size()];
        for (int i = 0; i < finalResult.size(); i++) {
            answer[index] = finalResult.get(i);
            index++;
        }
        return answer;
    }
}


class Song { //멤버 변수 : 고유번호 + 재생횟수
    int uniqueNumber;
    int playTime;

    public Song(int uniqueNumber, int playTime) {
        this.uniqueNumber = uniqueNumber;
        this.playTime = playTime;
    }
}
```

---

#### My logic & feedback

어려운 문제는 아니지만 상당히 짜증났던 문제...그래서 코드도 길고 더럽게 짜진 문제...

핵심은, Hash를 <String, Integer>로도 만들 수 있지만,

'장르별 재생횟수가 다르다' 라는 점을 이용해 <String, Integer>로도 만들수 있다는 점이다.

그리고 문제의 조건에 오름차순, 내림차순이 막 섞여 있어서 정렬이 굉장히 중요했던 문제..

다른 스터디원들은 어떻게 이 복잡한 문제를 깔끔하게 풀었을 지 궁금하다.