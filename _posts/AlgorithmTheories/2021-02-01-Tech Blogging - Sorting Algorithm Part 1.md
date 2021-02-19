---
layout: post
title: "[Algorithm] 정렬알고리즘 Part 1 - O(N^2)"
subtitle: "Bubble sort, selection sort, insertion sort"
background: '/img/bg_technology.jpg'
categories: technology-algorithm

---

정렬 알고리즘을 정리할 필요가 있을 것 같아 Part 1, Part 2, Part 3로 나누어서 정렬 알고리즘을 정리한다.

<br/>

정렬 알고리즘은 크게 수행시간이 O(N^2), O(NlogN), 그 외인 정렬(ex)기수정렬)로 나눌 수 있는데,

당연하게도 후자로 갈수록 더 빠르고, 구현이 좀 더 복잡한 정렬들이다.

<br/>

Part 1에서는 일반적으로 **O(N^2)의 시간복잡도**가 소요되는 

- 버블 정렬
- 선택 정렬
- 삽입 정렬

에 대해 포스팅하려한다.

<br/>

---

#### 1. 버블 정렬 - 거의 모든 상황에서 O(N^2)의 성능

구현이 가장 단순하지만, 그에 맞게 최악의 성능을 보여주는 정렬알고리즘이다.

다음과 같은 배열이 있을 때 : 

```java
int[] arr = {6, 5, 3, 1, 8, 7, 2, 4};
```

버블 정렬은 **자기자신과 그 다음 element를 비교하여 큰 것을 뒤로 계속해서 보내는 방식이다.**

예를 들어, 맨 처음 pass때는 6이 1 뒤로 가고, 8이 배열의 맨 끝으로 가고...하는 식이다.

위 배열의 버블정렬 결과는 다음과 같다.

```
1번째 : 5 3 1 6 7 2 4 8 
2번째 : 3 1 5 6 2 4 7 8 
3번째 : 1 3 5 2 4 6 7 8 
4번째 : 1 3 2 4 5 6 7 8 
5번째 : 1 2 3 4 5 6 7 8 
```

<br/>

버블정렬은 **한 pass가 끝날 때 마다 [배열의 길이- pass수] 인덱스의 element는 가장 큰 값으로 정렬된다**.

한 pass마다 가장 큰 것이 맨 뒤로 가기 때문이다. 가령,

```java
int[] arr = {8, 7, 6, 5, 4, 3, 2, 1};
```

위 배열의 버블정렬 결과는 다음과 같다.

```
1번째 : 7 6 5 4 3 2 1 8 
2번째 : 6 5 4 3 2 1 7 8 
3번째 : 5 4 3 2 1 6 7 8 
4번째 : 4 3 2 1 5 6 7 8 
5번째 : 3 2 1 4 5 6 7 8 
6번째 : 2 1 3 4 5 6 7 8 
7번째 : 1 2 3 4 5 6 7 8 
```

<br/>

**소스 :** 

```java
/* 가장 큰 원소를 맨 끝으로 보낸다 */
private void sort() {
    for (int i = 0; i < arr.length - 1; i++) {
        for (int j = 0; j < arr.length - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}
```

<br/>

---

#### 2. 선택 정렬 - 버블정렬보다는 빠른  O(N^2)의 성능

버블정렬이 자신과 자기 다음 element를 비교하여 자리를 바꾸는 정렬이라면,

선택정렬은 배열크기만큼 full scan하여 pass마다 가장 작은 값을 첫 번째, 두 번째, 세 번째...에 배치시키는 방법이다.

버블정렬에서는 pass수행마다 swap과정이 빈번하게 일어나는 반면, 

선택정렬은 인덱스만 저장해두고 pass가 끝나면 해당 인덱스(가장 작은 값)와 현재 인덱스만 swap해주면 된다.

**즉, 선택정렬은 '비교'과정은 버블정렬과 같지만, '교환'의 횟수가 현저히 적기 때문에 더 빠르다.**

<br/>

```java
int[] arr = {6, 5, 3, 1, 8, 7, 2, 4};
```

위 배열의 선택정렬 결과는 다음과 같다.

```
1번째 : 1 5 3 6 8 7 2 4 
2번째 : 1 2 3 6 8 7 5 4 
3번째 : 1 2 3 6 8 7 5 4 
4번째 : 1 2 3 4 8 7 5 6 
5번째 : 1 2 3 4 5 7 8 6 
6번째 : 1 2 3 4 5 6 8 7 
7번째 : 1 2 3 4 5 6 7 8 
```

버블정렬과는 다른 결과가 나옴을 확인할 수 있다.

<br/>

**소스 :** 

```java
/* 배열의 가장 작은 원소들을 순서대로 0, 1, 2 ... 인덱스에 배치한다 */
public void sort() {
    for (int i = 0; i < arr.length - 1; i++) {
        int minIdx = i;
        for (int j = i + 1; j < arr.length; j++) {
            if (arr[j] < arr[minIdx]) {
                minIdx = j;
            }
        }
        int temp = arr[i];
        arr[i] = arr[minIdx];
        arr[minIdx] = temp;
    }
}
```

<br/>

---

#### 3. 삽입 정렬 - O(N^2) 정렬 중 가장 나은 방식

삽입정렬은 특정 원소를 **현재까지 정렬된 subarray안의 올바른 위치에 끼워넣는 방식**이다.

마찬가지로 아래와 같은 배열이 있다고 하자.

```java
int[] arr = {6, 5, 3, 1, 8, 7, 2, 4};
```

우선 맨 첫 번째 element를 '이미 정렬된 subarray'라고 가정하자.

```java
int[] subarray = {6};
```

그리고 그 다음 element들이 들어올 때 마다 subarray의 올바른 위치에 element들을 끼워넣는 방식이다.

위 배열에서 이를 수행하면,

```
subarray = {6};
subarray = {5, 6};
subarray = {3, 5, 6};
subarray = {1, 3, 5, 6};
subarray = {1, 3, 5, 6, 8};
subarray = {1, 3, 5, 6, 7, 8};
subarray = {1, 2, 3, 5, 6, 7, 8};
subarray = {1, 2, 3, 4, 5, 6, 7, 8};
```

과 같은 형태로 정렬이 될 것이다.

구현은,

**subarray의 맨 마지막 element가 자신보다 작으면 pass하고 (subarray는 이미 정렬되어있으므로),** 

**아니라면 subarray의 끝 element부터 자기 자신과 값을 바꾸어나간다.**

<br/>

위 배열의 삽입정렬 결과는 다음과 같다.

```
1번째 : 5 6 3 1 8 7 2 4 
2번째 : 3 5 6 1 8 7 2 4 
3번째 : 1 3 5 6 8 7 2 4 
4번째 : 1 3 5 6 8 7 2 4 
5번째 : 1 3 5 6 7 8 2 4 
6번째 : 1 2 3 5 6 7 8 4 
7번째 : 1 2 3 4 5 6 7 8 
```

**소스 :**

```java
/* n번째 까지의 배열은 이미 정렬되어있다고 가정, 
 * n + 1 번째 원소가 들어올 때 마다 앞선 n개의 원소들 사이에서 
 * 자리를 찾아 끼워넣는다. */
public void sort() {
    for (int i = 1; i < arr.length; i++) {
        int val = arr[i];
        int idx = i;
        /* n번째 까지의 배열 중 마지막 원소가 비교대상보다 클 때만 for문 진입 */
        for (int j = i - 1; j >= 0 && arr[j] > val; j--) {
            arr[idx] = arr[j];
            idx = j;
        }
        arr[idx] = val;
    }
}
```

<br/>

나무위키에 의하면,

> 그밖에도 배열이 작을 경우에 역시 상당히 효율적이다. 일반적으로 빠르다고 알려진 알고리즘들도 배열이 작을 경우에는 대부분 삽입 정렬에 밀린다. 따라서 고성능 알고리즘들 중에서는 배열의 사이즈가 클때는O(nlogn) 알고리즘을 쓰다가 정렬해야 할 부분이 작을때 는 삽입 정렬로 전환하는 것들도 있다.

라고 한다.

삽입정렬은 구현도 어렵지 않고 간단하므로,

**만약 정렬을 빠르고 쉽게 구현해야 할 일이 있으면(기술 면접이라던가 손코딩이라던가...)**

**삽입정렬을 구현하는 것이 가장 바람직할것 같다.**

<br/>

---

#### 3개 정렬방식의 속도 차이 비교

크기가 30만인 배열을 정렬하는 데 버블 정렬, 선택 정렬, 삽입 정렬이 각각 몇 초의 시간이 걸리는지 측정했다.

Java의 Random 객체를 통해 30만개의 난수를 생성, 같은 배열을 정렬하는 데 걸리는 시간의 결과는 다음과 같다.

**테스트 소스 :**

```java
public class Test {
    public static void main(String[] args) {
        int N = 300000; // 배열 사이즈
        int arrListSize = 3; // 생성할 테스트 배열 개수
        ArrayList<int[]> arrList = new ArrayList<>();
        for (int i = 0; i < arrListSize; i++) { // 배열을 만들어 arrayList에 넣어준다.
            int[] arr = new int[N];
            arrList.add(arr);
        }
        Random random = new Random();
        for (int i = 0; i < N; i++) { // 난수를 생성하여 배열에 넣어준다.
            int randomNum = random.nextInt();
            for (int j = 0; j < arrListSize; j++) {
                arrList.get(j)[i] = randomNum;
            }
        }

        System.out.println("---정렬할 배열 원소의 개수 : " + N);

        int idx = 0;
        int startSec = (int) System.currentTimeMillis() / 1000;
        BubbleSort bubbleSort = new BubbleSort(arrList.get(idx));
        int endSec = (int) System.currentTimeMillis() / 1000;
        System.out.println("버블소트 소요 second : " + (endSec - startSec));
        idx++;

        startSec = (int) System.currentTimeMillis() / 1000;
        SelectionSort selectionSort = new SelectionSort(arrList.get(idx));
        endSec = (int) System.currentTimeMillis() / 1000;
        System.out.println("셀렉션소트 소요 second : " + (endSec - startSec));
        idx++;

        startSec = (int) System.currentTimeMillis() / 1000;
        InsertionSort insertionSort = new InsertionSort(arrList.get(idx));
        endSec = (int) System.currentTimeMillis() / 1000;
        System.out.println("인설션소트 소요 second : " + (endSec - startSec));
        idx++;
    }
}

```

<br/>

**결과**

```
---정렬할 배열 원소의 개수 : 300000
버블소트 소요 second : 142
셀렉션소트 소요 second : 38
인설션소트 소요 second : 9
```

<br/>



