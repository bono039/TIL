# 최소 공통 조상 (LCA : Lowest Common Ancestor)

> 🔗 출처 : https://gyoogle.dev/blog/algorithm/LCA.html
>
> <br/>
> LCA
> 
> 구현 방법

<br/>

## 최소 공통 조상
> 두 정점이 만나는 최초의 부모 정점을 찾는 것

* 공통 조상을 찾아야 하거나, 정점과 정점 사이의 이동거리/방문 경로를 저장해야 할 경우 사용
<br/>

### 구현 방법
```java
// 두 정점의 depth 확인하기
while(true) {
  if(depth가 일치)
    if(두 정점의 parent 일치?)
      LCA 찾음 (종료)
    else  // depth 불일치
      더 depth가 깊은 정점을 해당 정점의 parent 정점으로 변경 (depth 감소)
}

```

<br/>

### ➕ 연습문제
- [11437번: LCA](https://www.acmicpc.net/problem/11437)
- [11438번: LCA 2](https://www.acmicpc.net/problem/11438)
