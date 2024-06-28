# 비트마스크 (BitMask)

> 🔗 출처 : [비트마스크(BitMask)](https://gyoogle.dev/blog/algorithm/BitMask.html), [[알고리즘] 비트(Bit)와 비트마스크(BitMask) 정리 (Java)](https://loosie.tistory.com/238), [[java] 비트 마스크](https://hongjuzzang.github.io/bitmask/bit_mask/)
>
> <br/>
>
> 1. 정의
> 2. 장점
> 3. 비트 연산
> 4. 비트마스크를 이용한 집합 구현

<br/>

## 정의
> 정수의 2진수 표현을 자료구조로 쓰는 기법
* DP, 순열 등 배열 활용만으로 해결할 수 없을 때 사용

<br/>

## 장점
1. 수행 시간이 빠르다. → <code>O(1)</code>
2. 비트 연산자를 사용해 코드가 간결해진다.
3. <i><b>부분집합을 정수를 통해 나타낼 수 있다.</b></i> → 1: 포함O / 2: 포함X
4. 더 작은 메모리를 사용할 수 있다. (DP에서 매우 유용)

<br/>

## 비트 연산

|연산식|설명|
|:---|:---|
|<code>x << y</code>|x의 각 비트를 왼쪽으로 y만큼 이동. <br/> 오른쪽 빈자리는 0으로 채운다.|
|<code>x >> y</code>|x의 각 비트를 오른쪽으로 y만큼 이동. <br/> 왼쪽 빈자리는 <b>최상위 부호비트와 같은 값</b>으로 채운다.<br/> → 산술 자리이동(Arithmetic shift)|
|<code>x >>> y</code>|x의 각 비트를 오른쪽으로 y만큼 이동. <br/> 왼쪽 빈자리는 <b>0</b>으로 채운다. <br/> → 논리적 자리이동(Logical shift)|

<br/>

## 비트마스크를 이용한 집합 구현
* A : 10개의 집합의 상태를 나타내는 변수
  
|연산|예시|
|:---|:---|
|공집합 / 꽉 찬 집합 | <code>A=0</code> / <code>A = (1<<10) - 1</code> |
|원소 추가 | OR 연산 ⇒ <code>A \|= (1 << k)</code> |
|원소 삭제 | AND 연산 ⇒ <code>A &= ~(1 << k)</code> |
|원소 조회  | k번째 비트가 켜 있는지 확인 <br/>⇒ <code>if((A & (1 << k)) == (1 << k)) </code> |
|원소 토글 | 집합 A에 원소가 빠져있는 경우 추가하고, 들어있는 경우는 삭제 <br/> : XOR 연산 ⇒ <code>A ^= (1<<k)</code> |
|두 집합에 대해 연산|합집합 ⇒ <code>A \| B</code> <br/> 교집합 ⇒ <code>A & B</code> <br/> A에서 B를 뺀 차집합 ⇒ <code>A & (~B)</code> <br/> A와 B 중 하나에만 포함된 원소들의 집합 ⇒ <code>A ^ B </code>|
|집합의 크기 구하기|A에서 켜진 비트 수 구하기 <br/> : <code>Integer.bitCount(x);</code> <br/>또는<br/> int bitCnt(int x) { <br/> &nbsp;&nbsp;if(x == 0) return 0; <br/> &nbsp;&nbsp;return A%2 + bitCnt(A/2);<br/>}|
|최소 원소 찾기| 켜져 있는 비트 中 가장 오른쪽에 있는 비트 찾기 <br/> : <code>int first = A & (-A)</code>|
|최소 원소 지우기|<code>A &= (A-1)</code>|
|모든 부분 집합 순회하기|<code>for(int subset = A ; subset > 0 ; subset = ((subset - 1) & A)){}</code>|
<br/>

### 예제
배열 [1,2,3,4,5]에서 부분집합을 만들었을 때, 원소가 2개인 부분 집합 구하기
- 재귀 ver.
 ```java
    private staic void dfs(int[] arr, boolean[] chk, int idx, int cnt) {
      if(cnt == 2) {
        for(boolean b : chk) {
          if(b)
            System.out.print(b + " ");
        }
        System.out.println();
        return;
      }

      if(idx == chk.length)
        return;

      chk[idx] = true;
      dfs(arr, chk, idx+1, cnt+1);
      chk[idx] = false;
      dfs(arr, chk, idx+1, cnt);
    }
 ```
- 비트마스크 ver.
 ```java
    // 1을 arr의 크기 5만큼 왼쪽 shift
    for(int i = 0 ; i < (1 << arr.length) ; i++) {
      if(Integer.bitCount(i) == 2) {
        for(int j = 0 ; j < arr.length ; j++) {
          if((i & (1 << j)) > 0)  // i가 j번째 원소를 선택한 경우, j번째 원소 출력
            System.out.print(arr[j] + " ");
        }
        System.out.println();
      }
    }
 ```


<br/>

### ➕ 연습문제
- [1094번: 막대기](https://www.acmicpc.net/problem/1094)
- [1562번: 계단 수](https://www.acmicpc.net/problem/1562)
- [2098번: 외판원 순회](https://www.acmicpc.net/problem/2098)
- [2309번: 일곱 난쟁이](https://www.acmicpc.net/problem/2309)
