# 선택 정렬

> 🔗 출처 : https://gyoogle.dev/blog/algorithm/Selection%20Sort.html
>
>
> 1. 정의
> 2. 시간 복잡도
> 3. 공간 복잡도
> 4. 장단점

<br/>

## 정의
> 원소를 넣을 자리는 이미 정해져 있고, 그 위치에 어떤 원소를 넣을지 선택하는 알고리즘

(+ 버블 정렬보다 조금 더 빠름)

#### 예제 코드 (오름차순)
1. 주어진 배열 값 中 최솟값을 찾는다.
2. 최솟값을 맨 앞에 위치한 값과 교체한다.
3. 맨 처음 위치를 제외한 나머지 배열을 같은 방법으로 교체한다.

```java
void selectionSort(int[] arr) {
  int idxMin, temp;

  for (int i = 0 ; i < arr.length-1 ; i++) {    // 1. 위치(idx) 선택
    idxMin = i;

    for (int j = i+1 ; j < arr.length ; j++) {    // 2. idx의 값과 비교
      if (arr[j] < arr[idxMin]) {    // 3. 위치 갱신 (오름차순이므로)
        idxMin = j;
      }
    }

    // 4. swap(arr[idxMin], arr[i])
    temp = arr[idxMin];
    arr[idxMin] = arr[i];
    arr[i] = temp;
  }

  System.out.println(Arrays.toString(arr));
}

```

<br/>

## 시간 복잡도
<b>O(N^2)</b> : (n-1) + (n-2) + .... + 2 + 1 ⇒ n(n-1)/2

<br/>

## 공간 복잡도
<b>O(n)</b> - 주어진 배열 안에서 교환(swap)을 통해 정렬이 수행되므로

<br/>

## 장단점
* 장점
   * 버블 정렬처럼 단순한 알고리즘
   * 다른 메모리 공간 불필요 ⇒ 제자리(in-place) 정렬
   * 많은 교환이 일어나야 하는 자료 상태에서 비교적 효율적 (정렬을 위한 비교 횟수는 많으나, 버블 정렬에 비해 실제 교환 횟수는 적으므로)

* 단점
  * O(N^2)으로 비효율적인 시간 복잡도
  * <b>불안정 정렬</b>
