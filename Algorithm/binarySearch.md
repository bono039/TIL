# 이분 탐색

> 🔗 출처 : https://gyoogle.dev/blog/algorithm/Binary%20Search.html
>
>
> 1. 정의
> 2. 과정
> 3. 시간 복잡도

<br/>

## 정의
> 탐색 범위를 두 부분으로 분할하며 찾는 방식

<br/>

## 과정
1. 배열을 정렬한다.
2. left와 right로 mid 값을 설정한다. ⇒ <code>mid = (start + end) / 2</code>
3. mid와 내가 구하고자 하는 값을 비교한다.
   * 구할 값 > mid인 경우, left = mid + 1;
   * 구할 값 < mid인 경우, right = mid - 1;
4. 과정 1-3을 <code>left > right</code>가 될 때까지 계속 반복한다.

```java
public static int solution(int[] arr, int M) { // 배열 arr에서 M 찾기
	
    Arrays.sort(arr); // 정렬
	
    int left = 0;
    int right = arr.length - 1;
    int mid = 0;

    while (left <= right) {
        mid = (left + right) / 2;
        if (M == arr[mid]) {
            return mid;
        }
        else if (arr[mid] < M) {
            left = mid + 1;
        }
        else if (M < arr[mid]) {
            right = mid - 1;
        }
    }
}

```

<br/>

## 시간 복잡도
O(logN)
