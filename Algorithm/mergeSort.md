#  병합 정렬

> 🔗 출처 : https://gyoogle.dev/blog/algorithm/Merge%20Sort.html
>
>
> 1. 정의
> 2. 시간 복잡도
> 3. ETC.

<br/>

## 정의
> <b>분할 정복 (Divide and Conquer)</b>을 통해 주어진 배열을 정렬한다.

*_분할 정복_ : 큰 문제를 작은 문제 단위로 쪼개면서 해결해 나가는 방식

<br/>

#### 예제코드
요소를 쪼갠 후, 다시 합병하변서 정렬한다. (≒ 퀵 정렬)
<details>
  <summary>퀵 정렬과의 차이점</summary>
    * 퀵 정렬 : 우선 pivot을 통해 정렬(partition) → 영역을 쪼갬(quickSort) <br/>
    * 합병 정렬 : 영역을 쪼갤 수 있을만큼 쪼갬(mergeSort) → 정렬(merge)
</details>

<br/>

- <code>mergeSort</code>
```
public void mergeSort(int[] arr, int left, int right) {
    if(left < right) {
      int mid = (left + right) / 2;

      mergeSort(arr, left, mid);
      mergeSort(arr, mid + 1, right);
      mergeSort(arr, left, mid, right);
    }
}

```

<br/>

- <code>merge()</code> ⭐
  - 이미 <b>합병 대상인 두 영역이 각 영역에 대해 정렬되어 있으므로</b>, 단순히 두 배열을 <b>순차적으로 비교하며 정렬할 수 있다!</b>
     - <b>합병 정렬, 순차적</b> 비교로 정렬 진행하므로, <code><b>LinkedList</code>의 정렬이 필요할 때 사용하면 효율적</b>이다.
```java
public static void merge(int[] arr, int left, int mid, int right)
    int[] L = Arrays.copyOfRange(arr, left, mid+1);
    int[] R = Arrays.copyOfRange(arr, mid+1, right+1);

    int i = 0;
    int j = 0;
    int k = left;
    
    while(i < L.length && j < R.length) {
        if(L[i] <= R[j]) {
          arr[k] = L[i++];
        }
        else {
          arr[k] = R[j++];
        }
        k++;
    }

    while(i < L.length) {
      arr[k++] = L[i++];
    }
    while(j < R.length) {
      arr[k++] = R[j++];
    }
}

```

<br/>

## 시간 복잡도
- 최악 - <code><b>O(nlogn)</b></code>
- 최선 - <code><b>Ω(nlogn)</b></code>
- 평균 : <code><b>Θ(nlogn)</b></code>


<br/>

## ETC.
* 빠른 정렬에 속함
* 퀵 정렬과 달리 <b>안정 정렬</b>
