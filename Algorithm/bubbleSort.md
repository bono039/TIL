# 버블 정렬

> 🔗 출처 : https://gyoogle.dev/blog/algorithm/Bubble%20Sort.html
>
>
> 1. 정의
> 2. 시간 복잡도
> 3. 공간 복잡도
> 4. 장단점

<br/>

## 정의
> 서로 인접한 두 원소의 대소를 비교하고, 조건에 맞지 않으면 자리를 교환하면 정렬하는 알고리즘



#### 예제 코드
```java
void bubbleSort(int[] arr) {
  int temp = 0;
  
  for(int i = 0; i < arr.length; i++) {       // 제외될 원소 갯수
    for(int j = 1 ; j < arr.length - i; j++) { // 원소 비교할 idx
      if(arr[j-1] > arr[j]) {             // 현재 가리키고 있는 두 원소의 대소 비교하고 swap
        // swap(arr[j-1], arr[j])
        temp = arr[j-1];
        arr[j-1] = arr[j];
        arr[j] = temp;
      }
    }
  }
  System.out.println(Arrays.toString(arr));
}

```

<br/>

## 시간 복잡도
<b>O(N^2)</b> - 2개의 원소를 비교하므로

<br/>

## 공간 복잡도
<b>O(n)</b> - 주어진 배열 안에서 교환(swap)을 통해 정렬이 수행되므로

<br/>

## 장단점
* 장점
   * 구현이 간단하고, 코드가 직관적
   * 다른 메모리 공간 불필요 ⇒ 제자리(in-place) 정렬
   * 안정(stable) 정렬

* 단점
  * O(N^2)으로 최악의 시간 복잡도. 비효율적
  * 정렬되지 않은 원소를 정렬하고자 할 때 교환 연산(swap) 많이 발생
