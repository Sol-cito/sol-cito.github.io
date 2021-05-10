---
layout: post
title: "[Algorithm] 프로그래머스 지형이동 (Level 4)"
subtitle: Implementation, heap solution
background: '/img/bg_technology.jpg'
categories: technology-algorithm
---

### 문제

N x N 크기인 정사각 격자 형태의 지형이 있습니다. 각 격자 칸은 1 x 1 크기이며, 숫자가 하나씩 적혀있습니다. 격자 칸에 적힌 숫자는 그 칸의 높이를 나타냅니다.

이 지형의 아무 칸에서나 출발해 모든 칸을 방문하는 탐험을 떠나려 합니다. 칸을 이동할 때는 상, 하, 좌, 우로 한 칸씩 이동할 수 있는데, 현재 칸과 이동하려는 칸의 높이 차가 height 이하여야 합니다. 높이 차가 height 보다 많이 나는 경우에는 사다리를 설치해서 이동할 수 있습니다. 이때, 사다리를 설치하는데 두 격자 칸의 높이차만큼 비용이 듭니다. 따라서, 최대한 적은 비용이 들도록 사다리를 설치해서 모든 칸으로 이동 가능하도록 해야 합니다. 설치할 수 있는 사다리 개수에 제한은 없으며, 설치한 사다리는 철거하지 않습니다.

각 격자칸의 높이가 담긴 2차원 배열 land와 이동 가능한 최대 높이차 height가 매개변수로 주어질 때, 모든 칸을 방문하기 위해 필요한 사다리 설치 비용의 최솟값을 return 하도록 solution 함수를 완성해주세요.

#### 제한사항
- land는 N x N크기인 2차원 배열입니다.
- land의 최소 크기는 4 x 4, 최대 크기는 300 x 300입니다.
- land의 원소는 각 격자 칸의 높이를 나타냅니다.
- 격자 칸의 높이는 1 이상 10,000 이하인 자연수입니다.
- height는 1 이상 10,000 이하인 자연수입니다.

#### 입출력 예
land	height	result
<br/>
[[1, 4, 8, 10], [5, 5, 5, 5], [10, 10, 10, 10], [10, 10, 10, 20]]	3	15
<br/>
[[10, 11, 10, 11], [2, 21, 20, 10], [1, 20, 21, 11], [2, 1, 2, 1]]	1	18

#### 입출력 예 설명
입출력 예 #1

각 칸의 높이는 다음과 같으며, 높이차가 3 이하인 경우 사다리 없이 이동이 가능합니다.

![image](https://grepp-programmers.s3.amazonaws.com/files/production/c08b7af3db/5efe34cb-1e69-4474-8e0f-b6929184ebdd.png)

위 그림에서 사다리를 이용하지 않고 이동 가능한 범위는 같은 색으로 칠해져 있습니다. 예를 들어 (1행 2열) 높이 4인 칸에서 (1행 3열) 높이 8인 칸으로 직접 이동할 수는 없지만, 높이가 5인 칸을 이용하면 사다리를 사용하지 않고 이동할 수 있습니다.

따라서 다음과 같이 사다리 두 개만 설치하면 모든 칸을 방문할 수 있고 최소 비용은 15가 됩니다.

높이 5인 칸 → 높이 10인 칸 : 비용 5
높이 10인 칸 → 높이 20인 칸 : 비용 10

#### 입출력 예 #2

각 칸의 높이는 다음과 같으며, 높이차가 1 이하인 경우 사다리 없이 이동이 가능합니다.

![image](https://grepp-programmers.s3.amazonaws.com/files/production/5bfffc0d72/af5db829-8ea1-4f4c-a5a8-ed11e029d135.png)

위 그림과 같이 (2행 1열) → (1행 1열), (1행 2열) → (2행 2열) 두 곳에 사다리를 설치하면 설치비용이 18로 최소가 됩니다.

[문제 링크](https://programmers.co.kr/learn/courses/30/lessons/62050)

---

#### My solution (Python)

```python
import heapq, sys
from collections import deque
sys.setrecursionlimit(100000)

# bfs로 구역을 각각 dictionary에 저장하고, landMap에 구역 그룹에 해당하는 번호를 새긴다.
def bfs(x, y, height, landNum, landDic, visit, land, landMap):
    que = deque([[x, y]])
    visit[x][y] = True
    while que:
        pop = que.pop()
        landMap[pop[0]][pop[1]] = landNum
        for dx, dy in [1, 0], [0, 1], [-1, 0], [0, -1]:
            if 0 <= pop[0] + dx < len(land) and 0 <= pop[1] + dy < len(land[0]) and abs(
                    land[pop[0]][pop[1]] - land[pop[0] + dx][pop[1] + dy]) <= height and not \
                    visit[pop[0] + dx][pop[1] + dy]:
                visit[pop[0] + dx][pop[1] + dy] = True
                que.appendleft([pop[0] + dx, pop[1] + dy])
                landDic.get(landNum).append([pop[0] + dx, pop[1] + dy])

# heap으로 각 구역을 잇는 최소비용 사다리를 찾는다.
def findLadder(land, landNum, landDic, heap, visit, landMap, prevSet, height):
    prevSet.add(landNum)
    for node in landDic.get(landNum):
        for dx, dy in [1, 0], [0, 1], [-1, 0], [0, -1]:
            nx, ny = node[0] + dx, node[1] + dy
            if 0 <= nx < len(land) and 0 <= ny < len(land[0]) and not visit[nx][ny] and landMap[node[0]][node[1]] != \
                    landMap[nx][ny] and abs(land[node[0]][node[1]] - land[nx][ny]) > height:
                heapq.heappush(heap, [abs(land[node[0]][node[1]] - land[nx][ny]), landMap[nx][ny]])
    res = 0
    while heap:
        heapPop = heapq.heappop(heap)
        if heapPop[1] in prevSet: continue
        heapPop[0]
        res += heapPop[0]
        res += findLadder(land, heapPop[1], landDic, heap, visit, landMap, prevSet, height)
    return res


def solution(land, height):
    landNum, landDic, landMap = 1, {}, [[0] * len(land) for _ in range(len(land[0]))]
    visit = [[False] * len(land[0]) for _ in range(len(land))]
    for i in range(len(land)):
        for j in range(len(land[0])):
            if not visit[i][j]:
                landDic[landNum] = [[i, j]]
                bfs(i, j, height, landNum, landDic, visit, land, landMap)
                landNum += 1
    heap = []
    visit = [[False] * len(land[0]) for _ in range(len(land))]
    answer = findLadder(land, 1, landDic, heap, visit, landMap, set([]), height)
    return answer
```



---

#### My logic & Feedback

이 문제는 약 1년전에 풀다가 시간초과로 실패한 문제였는데(당시엔 Java로 풀이),

다시 시도해서 풀이에 성공하였다.

문제는 크게 두 가지 구현이 필요한데,

1. 사다리 없이 이동할 수 있는 지역을 묶는 것
2. 각 구역 별 최소비용의 사다리(높이 차가 최소인)를 찾는 것

이다.

과거에는 1번은 O(N)으로 구현하였으나, 2번에서 O(N^2)으로 구현하여 시간초과가 났던 것 같다.

최악의 경우 노드가 9만개이기 때문에, N^2은 약 8억번의 연산이 필요하기 때문이다.

어떤 두 구역의 최소비용 사다리를 찾은 후, 다음 번 구역과의 사다리 비용을 계산할 때 

이미 이어진 두 구역과 새로 이을 구역의 모든 Node를 탐색하였기 때문이다.

이번에는 새로운 구역과 이미 이어진 구역 사이의 최소비용 사다리를 찾을 때 heap을 사용하였다.

각 노드가 지닌 다른 구역과의 사다리 값과 노드 좌표를 heap에 담으면,

새로운 구역과의 사다리가 필요할 때 heap의 최상단에 있는 노드가 가장 최소비용을 가진 노드일 것이므로

O(logN)의 시간이 소요되기 때문이다.

구현이 조금 빡빡하긴 했지만, 어쨌든 시간초과 없이 풀이에 성공해서 기쁘다.