#  힙 정렬

> 🔗 출처 : https://gyoogle.dev/blog/algorithm/Heap%20Sort.html
>
>
> 1. 정의
> 2. 과정
> 3. 시간 복잡도
> 4. 유용한 경우

<br/>

## 정의
> <b>완전 이진 트리</b>를 기본으로 하는 Heap 자료구조를 기반으로 한 정렬 방식

*_완전 이진 트리_ : 삽입할 때 왼쪽부터 차례대로 추가하는 이진 트리
- 최대값을 1개씩 뽑으면서 정렬

<br/>

## 과정
1. 최대 힙을 구성한다.
2. 현재 Heap 루트에 최댓값이 존재한다. Root의 값을 마지막 요소와 바꾼 후, Heap의 사이즈를 하나 줄인다.
3. Heap의 사이즈가 1보다 크면 위 과정을 반복한다.

<br/>

#### 예제코드 

```java
public void heapSort(int[] arr) {
    int n = arr.length;
    
    // max heap 초기화
    for (int i = n/2-1 ; i >= 0 ; i--){
        heapify(arr, n, i); // (1)
    }
    
    // extract 연산
    for (int i = n-1 ; i > 0 ; i--) {
        swap(arr, 0, i); 
        heapify(arr, i, 0); // (2)
    }
}

```

(1) heapify
   - 일반 배열을 Heap으로 구성한다.
   - 자식 노드로부터 부모 노드 비교
   - n/2-1 부터 0까지 인덱스 도는 이유 : 부모 노드 인덱스 기준 왼쪽 자식 노드는 (i2+1), 오른쪽 자식 노드는 (i2+2)이므로
<br/>

(2) heapify
  - 요소가 1개 제거된 후 다시 최대 힙을 구성하기 위함
     - <b>다시 최대 힙을 구성할 때까지</b> 부모노드와 자식노드 swap하며 재귀 진행
  - Root를 기준으로 진행 (extract 연산 처리를 위해)
```java
public void heapify(int arr[], int n, int i) {
    int p = i;
    int l = i*2 + 1;
    int r = i*2 + 2;
    
    // 왼쪽 자식노드
    if (l < n && arr[p] < arr[l]) {
        p = l;
    }
    // 오른쪽 자식노드
    if (r < n && arr[p] < arr[r]) {
        p = r;
    }
    
    // 부모노드 < 자식노드
    if(i != p) {
        swap(arr, p, i);  // 부모 노드와 자식 노드 swap
        heapify(arr, n, p);
    }
}
```
<br/>

<details>
  <summary>전체 코드</summary>
  
    private void solve() {
        int[] array = { 230, 10, 60, 550, 40, 220, 20 };
     
        heapSort(array);
     
        for (int v : array) {
            System.out.println(v);
        }
    }
     
    public static void heapify(int array[], int n, int i) {
        int p = i;
        int l = i * 2 + 1;
        int r = i * 2 + 2;

        // 왼쪽 자식노드
        if (l < n && array[p] < array[l]) {
            p = l;
        }

        // 오른쪽 자식노드
        if (r < n && array[p] < array[r]) {
            p = r;
        }

        // 부모노드 < 자식노드
        if (i != p) {
            swap(array, p, i);  // 부모 노드와 자식 노드 swap
            heapify(array, n, p);
        }
    }
     
    public static void heapSort(int[] array) {
        int n = array.length;
     
        // max heap 초기화
        for (int i = n/2 -1; i >= 0; i--) {
            heapify(array, n, i);
        }
     
        // extract 연산
        for (int i = n-1; i > 0; i--) {
            swap(array, 0, i);
            heapify(array, i, 0);
        }
    }
     
    public static void swap(int[] array, int a, int b) {
        int temp = array[a];
        array[a] = array[b];
        array[b] = temp;
    }

</details>

<br/>

## 시간 복잡도
- 최악 - <code><b>O(nlogn)</b></code>
- 최선 - <code><b>Ω(nlogn)</b></code>
- 평균 : <code><b>Θ(nlogn)</b></code>


<br/>

## 유용한 경우
* 최댓값/최솟값을 구할 때
    * 최소/최대 Heap의 루트 값이므로 1번의 Heap 구성을 통해 구할 수 있음
* 최대 k만큼 떨어진 요소들을 정렬할 때
    * 삽입 정렬보다 결과 더욱 개선 가능
