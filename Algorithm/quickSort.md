#  퀵 정렬

> 🔗 출처 : https://gyoogle.dev/blog/algorithm/Quick%20Sort.html
>
>
> 1. 정의
> 2. 과정
> 3. 시간 복잡도
> 4. 공간 복잡도
> 5. 장단점
> 6. 결론

<br/>

## 정의
> <b>분할 정복 (Divide and Conquer)</b>을 통해 주어진 배열을 정렬한다.

*_분할 정복_: 문제를 작은 2개의 문제로 분리하고 각각 해결한 후, 결과를 모아 원래의 문제를 해결하는 전략
<br/>
- vs 합병 정렬: 비균등 분할

<br/>

## 과정
1. 배열 내 원소 1개(<code>pivot</code>)를 고른다.
2. 피벗을 기준으로 배열을 둘로 나눈다. (=<b>_분할_</b> )
   - 피벗 앞 : 피벗보다 값이 작은 모든 원소들
   - 피벗 뒤 : 피벗보다 값이 큰 모든 원소들
3. 분할된 2개의 작은 배열에 대해 <b>재귀적</b>으로 이 과정을 반복한다.

<br/>

#### 예제코드
- 정복 (Conquer)
  - 부분 배열을 정렬한다. 부분 배열의 크기가 충분히 작지 않다면, 순환 호출을 이용해 다시 분할 정복한다.
```java
public void quickSort(int[] arr, int left, int right) {
    if(left >= right) return;
    
    // 분할 
    int pivot = partition(); 
    
    // 피벗은 제외한 2개의 부분 배열을 대상으로 순환 호출
    quickSort(arr, left, pivot-1);  // 정복 (Conquer)
    quickSort(arr, pivot+1, right); // 정복 (Conquer)
}

```

<br/>

- 분할 (Conquer)
  - pivot을 기준으로 입력 배열을 비균등하게 2개의 부분 배열로 분할한다.<br/>
    → 피벗 중심 [왼쪽/오른쪽] : 피벗보다 [작은/큰] 요소들
    
```java
public int partition(int[] arr, int left, int right) {
    /**
    // 최악의 경우, 개선 방법
    int mid = (left + right) / 2;
    swap(array, left, mid);
    */
    
    int pivot = arr[left]; // 가장 왼쪽값을 피벗으로 설정
    int i = left, j = right;
    
    while(i < j) {
        while(pivot < arr[j]) {
            j--;
        }
        while(i < j && pivot >= arr[i]){
            i++;
        }
        swap(arr, i, j);
    }
    arr[left] = array[i];
    arr[i] = pivot;
    
    return i;
}

```

<br/>

## 시간 복잡도
- 최악 - <code><b>T(n) = O(n^2)</b></code>
  * WHEN? 배열이 오름차순/내림차순 정렬되어 있는 경우
  * 비교 횟수 (= 순환 호출의 깊이) : [<code>n</code>](https://github.com/GimunLee/tech-refrigerator/raw/master/Algorithm/resources/quick-sort-003.png)
  * 각 순환 호출 단계에서의 비교 연산 : <code>n</code>
- 최선 - <code><b>T(n) = O(nlog₂n)</b></code>
  * 비교 횟수 (= 순환 호출의 깊이) : [<code>log₂n</code>](https://github.com/GimunLee/tech-refrigerator/raw/master/Algorithm/resources/quick-sort-002.png)
  * 각 순환 호출 단계에서의 비교 연산 : <code>n</code>
- 평균 : <code>T(n) = O(nlog₂n)</code>


<br/>

## 공간 복잡도
<b>O(n)</b> - 주어진 배열 안에서 교환(swap)을 통해 정렬이 수행되므로

<br/>

## 장단점
* 장점
   * 가장 빠른 정렬 알고리즘! (불필요한 데이터 이동 줄이고 먼 거리 데이터 교환할 뿐만 아니라, 한 번 결정된 pivot들이 추후 연산에서 제외되므로)
   * 다른 메모리 공간 불필요 ⇒ 제자리(in-place) 정렬

* 단점
  * <b>불안정 정렬</b>
  * 정렬된 배열에서는 퀵 정렬 시 오히려 수행시간이 더 오래 걸린다. (∵ 불균형 분할)

<br/>

## 결론
> 퀵 정렬은 평균적으로 가장 빠른 정렬 알고리즘이고, 효율적이다!
