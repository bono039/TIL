# 비트마스크 (BitMask)

> 🔗 출처 : [비트마스크(BitMask)](https://gyoogle.dev/blog/algorithm/BitMask.html), [[알고리즘] 비트(Bit)와 비트마스크(BitMask) 정리 (Java)](https://loosie.tistory.com/238), [[java] 비트 마스크](https://hongjuzzang.github.io/bitmask/bit_mask/)
>
> <br/>
>
> 1. 정의
> 2. 장점
> 3. 비트마스크를 이용한 집합 구현

<br/>

## 정의
> 정수의 2진수 표현을 자료구조로 쓰는 기법

<br/>

## 장점
1. 수행 시간이 빠르다. → <code>O(1)</code>
2. 비트 연산자를 사용해 코드가 간결해진다.
3. <i><b>부분집합을 정수를 통해 나타낼 수 있다.</b></i> → 1: 포함O / 2: 포함X
4. 더 작은 메모리를 사용할 수 있다. (DP에서 매우 유용)

<br/>

## 비트마스크를 이용한 집합 구현
* A : 10개의 집합의 상태를 나타내는 변수
  
|연산|예시|
|:---|:---|
|공집합 / 꽉 찬 집합 | <code>A=0</code> / <code>A = (1<<10) - 1</code> |
|원소 추가 | OR 연산 ⇒ <code>A \|= (1 << k)</code> |
|원소 삭제 | AND 연산 ⇒ <code>A &= ~(1 << k)</code> |
|원소 조회 | <code>if((A & (1 << k)) == (1 << k)) </code> |
<br/>

### ➕ 연습문제
- [1094번: 막대기](https://www.acmicpc.net/problem/1094)
- [1562번: 계단 수](https://www.acmicpc.net/problem/1562)
- [2098번: 외판원 순회](https://www.acmicpc.net/problem/2098)
- [2309번: 일곱 난쟁이](https://www.acmicpc.net/problem/2309)
