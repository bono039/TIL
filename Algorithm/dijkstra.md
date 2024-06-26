# 다익스트라 (Dijkstra)

> 🔗 출처 : https://gyoogle.dev/blog/algorithm/Dijkstra.html
>
> <br/>
>
> 다익스트라
><br/>
> 
> 시간 복잡도
> 
> 구현 방법

<br/>

## 다익스트라
> 가중치가 있는 그래프에서 <b>출발 정점에서 모든 정점 간</b> 최단 거리 탐색 알고리즘 (DP 활용)

<img src="https://upload.wikimedia.org/wikipedia/commons/5/57/Dijkstra_Animation.gif">

⚠️ 가중치는 항상 <b>양수</b>여야 한다.

<br/>

## 시간 복잡도
- 인접 행렬로 구현 시 : <code>O(N^2)</code>
- 인접 리스트로 구현 시 : <code>O(E logV)</code> ⇒ 우선순위 큐 ☑️

<br/>

## 구현 방법
<b>📍 필요 자료구조</b>
- 노드 객체 → <code>(정점 번호, 가중치) 형태</code>
- <b>인접 리스트</b>
- 출발점부터 다른 정점까지의 <b>최단 거리 저장 배열</b>
  * 출발점 → 0
  * 그 외 다른 정점 → 적당히 큰 값 or ∞
- 해당 정점 <b>방문 여부 저장 배열</b>
- (인접 리스트) 우선순위 큐 <code>PriorityQueue<Node></code> → 사용자 정의 정렬

<br/>

<b>📍 과정</b>
1. 정점 번호와 가중치를 저장하는 Node 객체를 생성한다.
```java
class Node implements Comparable<Node>{
    int node, value;
    
    Node(int node, int value) {
        this.node = node;
        this.value = value;
    }

    // 거리(가중치) 기준 오름차순 정렬
    @Override
    public int compareTo(Node e) {
        return value - e.value;
    }
}
```

2. <b>인접 리스트</b>로 그래프를 구현한다.
```java
ArrayList<Node>[] graph = new ArrayList[V+1];
for(int i = 1 ; i <= V ; i++) {
  graph[i] = new ArrayList<>();
}
```
2. 최단 거리 배열 값은 무한대로 초기화한다.
```java
int dist = new int[V+1];
Arrays.fill(dist, Integer.MAX_VALUE);  // 문제에 따라 최댓값 달라짐
```

3. 시작 정점과 연결된 정점들의 최단 거리 값을 갱신한다.
```java
private static void dijkstra(int x) {
    PriorityQueue<Node> pq = new PriorityQueue<>();
    pq.add(new Node(x, 0));   // 큐에 탐색 시작점 추가

    dist[start] = 0; // 시작 정점의 최단 거리는 0

    while(!pq.isEmpty()) {
        Node now = pq.poll();

        if(visited[now.node])  continue;  // 방문한 정점은 pass
        visited[now.node] = true;  // 정점 방문 처리

        // 🔔 연결 노드 거리 리스트 값 > 선택 노드의 거리 리스트 값 + 에지 가중치인 경우, 업데이트
        for(Node next : A[now.node]) {
            if(dist[next.node] > dist[now.node] + next.cost) {  // 더 짧은 경로로 이동
                dist[next.node] = dist[now.node] + next.cost;
                pq.add(new Node(next.node, dist[next.node]));  // 연결된 노드를 우선순위 큐에 추가
            }
        }
    }        
}
```

<br/>

### ➕ 연습문제
- [1504번: 특정한 최단 경로](https://www.acmicpc.net/problem/1504)
- [1753번: 최단경로](https://www.acmicpc.net/problem/1753)
- [1916번: 최소비용 구하기](https://www.acmicpc.net/problem/1916)
- [18352번: 특정 거리의 도시 찾기](https://www.acmicpc.net/problem/18352)
