# 계수 정렬
> 1. 정의
> 2. 시간 복잡도
> 3. 장단점
> 4. 참고

<br/>

## 정의
> 배열 내에 특정한 값이 몇 번 등장했는지에 따라 정렬하는 방법
- 각 배열 원소끼리 직접 비교하는 것이 아닌, 인덱스를 갖고 위치를 찾아나감


#### 예제 코드
```java
int A[5]; 		// [5, 4, 3, 2, 1]
int sorted_arr[5];

// 과정 1) counting 배열의 사이즈를 최대값 5가 담기도록 크게 잡기
int counting[6];	// 단점 : counting 배열의 사이즈의 범위를 가능한 값의 범위만큼 크게 잡아야 하므로, 비효율적.

// 과정 2) counting 배열의 값 증가해주기.
for (int i = 0 ; i < A.length ; i++) {
    counting[A[i]]++;
}

// 과정 3) counting 배열을 누적합으로 만들기.
for (int i = 1 ; i < counting.length ; i++) {
    counting[i] += counting[i-1];
}

// 과정 4) 배열 뒤쪽부터 돌면서, 해당하는 값의 인덱스에 값 넣어주기.
for (int i = A.length - 1 ; i >= 0 ; i--) {
    counting[A[i]]--;
    sorted_arr[counting[A[i]]] = A[i];
}
```

<br/>
  
 ## 시간 복잡도
- <code><b>O(n + k)</b></code> ⇒ k는 배열 내 최댓값

<br/>

## 장단점
* 장점
   * O(N)의 시간 복잡도
   * 최댓값이 작을수록 정렬에 유리

* 단점
  * 수열의 길이보다 수의 범위가 극단적으로 크면 메모리 낭비 심함

<br/>

## 참고
- [계수 정렬(Counting sort)](https://gyoogle.dev/blog/algorithm/Counting%20Sort.html)
- [[알고리즘 정리]계수정렬(Counting Sort)](https://jeonyeohun.tistory.com/103)
- [카운팅 정렬/계수 정렬(Counting Sort)](https://velog.io/@wndudrla1011/interview-algorithm-sort-counting)
