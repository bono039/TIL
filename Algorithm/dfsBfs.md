# DFS & BFS

> 🔗 출처 : https://gyoogle.dev/blog/algorithm/DFS%20&%20BFS.html
>
> DFS
> 
> BFS

<br/>

## DFS
> 루트 노드 / 임의 노드에서 <b>다음 브랜치로 넘어가기 전, 해당 브랜치를 모두 탐색</b>하는 방법
- <b>스택/재귀함수</b>로 구현 (LIFO)
- 모든 경로 방문해야 할 경우 적합

<br/>

### 시간 복잡도
- 인접 행렬 : <code>O(V^2)</code>
- 인접 리스트 : <code>O(V+E)</code>

<br/>

#### 예제 코드

```java
public static void DFS(int node) {
    // 1) 종료 조건 : 현재 노드가 이미 방문한 노드인 경우
    if(visited[node]) return;

    // 현재 연결 노드 中 방문하지 않은 노드인 경우
    visited[node] = true;

    // 2) 재귀
    for(int i : A[i]) {
        if(!visited[i]) {
            DFS(i);
        }
    }
}

```

<br/>

## BFS
> 루트 노드 / 임의 노드에서 <b>인접한 노드부터 먼저 탐색</b>하는 방법
- <b>큐</b>로 구현 (FIFO)
- 최단 경로 탐색에 적합

<br/>

### 시간 복잡도
- 인접 행렬 : <code>O(V^2)</code>
- 인접 리스트 : <code>O(V+E)</code>

<br/>

#### 예제 코드

```java
public static void BFS(int node) {
    visited[node] = true;
    
    Queue<Integer> q = new LinkedList<>();
    q.add(node);

    while(!q.isEmpty()) {
        int now_node = q.poll();

        for(int next : graph[now_node]) {  // 인접한 노드 탐색
            if(!visited[next]) {
                visited[next] = true;
                q.add(next);  // 큐에 추가해 마저 탐색
            }
        }
    }
}

```

<br/>

## ➕ 연습문제
[1260번: DFS와 BFS](https://www.acmicpc.net/problem/1260)
