# 최장 증가 부분 수열 (LIS : Longest Increasing Sequence)

> 🔗 출처 : https://gyoogle.dev/blog/algorithm/LIS.html, https://chanhuiseok.github.io/posts/algo-49/
>
> <br/>
> LIS
> 
> 구현 방법 (시간 복잡도)

<br/>

## 최장 증가 부분 수열
> 가장 긴 증가하는 부분 수열

<br/>

### 구현 방법 (시간 복잡도)
1. DP : <code>O(N^2)</code>
   * <code>dp[i]</code> : i번째 인덱스에서 끝나는 최장 증가 부분 수열의 길이
2. 이분 탐색 : <code>O(NlogN)</code>
   * 배열의 길이가 최대 10만이라 N^2으로 해결할 수 없는 경우
   * 정확한 LIS를 구할 순 없으나, 길이 구할 때 유용
   * 탐색 대상 : LIS 형태 유지하기 위해 숫자가 들어갈 위치
<br/>

#### 예제 코드 1. DP를 활용한 LIS

```java
int[] arr = {7, 2, 3, 8, 4, 5};
int[] dp = new int[arr.length];  // LIS 저장 배열

for(int i = 1 ; i < dp.length ; i++) {
    dp[i] = 1;

    for(int j = 0 ; j < i ; j++) {
        if(arr[j] < arr[i]) {
            dp[i] = Math.max(dp[i], dp[j] +1);  // 더 큰 값으로 dp[i] 값 없데이트
        }
    }
}

for (int i = 0; i < dp.length; i++) {
    max = Math.max(max, dp[i]);
}

// 저장된 dp 배열 값 : [0, 0, 1, 2, 2, 3]
// LIS : dp배열에 저장된 값 중 최대 값 + 1

```

<br/>

#### 예제 코드 2. 이분 탐색을 활용한 LIS (예제 : BOJ 11053번)

```java
import java.util.*;
import java.io.*;
 
public class Main {
    static int[] A, LIS;
    
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        
        int N = Integer.parseInt(br.readLine());
        A = new int[N];
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        for(int i = 0 ; i < N ; i++) {
            A[i] = Integer.parseInt(st.nextToken());
        }
        
        LIS = new int[N];
        LIS[0] = A[0];
        int end = 0;
        
        for(int i = 1 ; i < N ; i++) {
            // 원 배열 탐색 중 더 작은 숫자라면 그대로 이어붙임
            if(LIS[end] > A[i]) {
                LIS[++end] = A[i];
            }
            else {
                int targetIdx = binarySearch(end, i);  // LIS 유지하고자 숫자가 들어갈 위치 탐색
                LIS[targetIdx] = A[i];
            }
        }
        
        System.out.println(end + 1);
    }
    
    private static int binarySearch(int end, int idx) {
        int start = 0;
        
        while(start <= end) {
            int mid = (start + end) / 2;
            
            if(LIS[mid] == A[idx])       return mid;
            else if(LIS[mid] > A[idx])   start = mid + 1;
            else if(LIS[mid] < A[idx])   end = mid - 1;
        }
        
        return start;   // 일치값 찾지 못 했을 때, 적절한 위치 반환
    }
```
<br/>

### ➕ 연습문제
- DP - [2565번: 전깃줄](https://www.acmicpc.net/problem/2565)
- 이분탐색 - [2352번: 반도체 설계](https://www.acmicpc.net/problem/2352)
