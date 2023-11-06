# Shortest Path Algorithms

- **최단 경로 알고리즘(Shortest Path Algorithms)**은 그래프에서 하나의 정점에서 다른 정점으로 가는 가장 짧은 경로의 **길이**를 구하는 알고리즘이다.



## 최단 경로 알고리즘의 종류

1. 너비 우선 탐색(BFS): 가중치가 없거나 모두 같은 그래프에 적절
2. 다익스트라 알고리즘(Dijkstra Algorithm): 가중치에 음이 없는 그래프, 정점의 개수가 많을 때 적절
3. 벨만-포드 알고리즘(Bellman-ford-Moore Algorithm): 가중치에 음이 있는 그래프에 적절
4. 플로이드-워셜 알고리즘(Floyd-Warshall Algorithm): 정점의 개수가 적을 때 적절

### 단일 시작점과 모든 쌍 알고리즘

최단 경로 알고리즘은 구하는 최단 거리의 범위에 따라 나눌 수 있다.

1. **단일 시작점 알고리즘**: 하나의 정점에서 다른 모든 정점까지 가는 최단 거리를 구한다. `V` 크기의 배열.
   - 예: BFS, 다익스트라 알고리즘, 벨만 포드 알고리즘
2. **모든 쌍 알고리즘**: 모든 정점의 쌍에 대하여 최단 거리를 구한다. `V * V`크기의 배열.
   - 예: 플로이드-워셜 알고리즘

### DFS로는 왜 최단 거리를 구할 수 없는가?

DFS로 최단 거리를 구할 수 없는 것은 아니다. 다만 모든 경우에서 최단 거리를 구할 수 있다고 보장하지 못한다.

주어진 그래프가 임의의 정점 A에서 B로 가는 경로가 하나만 있는 그래프라면, 즉 사이클이 없는 그래프(트리)라면 DFS로 최단 거리를 구할 수 있다. (애초에 경로가 하나이기 때문에 해당 경로가 최단 거리가 된다.)

그래프가 트리가 아니라고 하자. 즉, 임의의 정점 A에서 B로 가는 경로가 하나 이상이라고 하자. DFS는 깊이 우선 탐색이므로 순회하던 중 B에 방문하면 B로 가는 더 짧은 경로가 있었다해도 다시는 B에 가지 않을 것이다.

이와 달리 BFS는 너비 우선 탐색이라 B에 도착하면 가장 짧은 경로로 온 것이다. (단, 그래프의 모든 간선의 가중치는 없거나 동일하다.)

### BFS로 최단 거리 구하기

[BFS.md - 노드의 레벨 구하기 & 두 노드 사이의 최단거리 구하기](https://github.com/leegwae/algorithms/blob/main/bfs.md#%EB%85%B8%EB%93%9C%EC%9D%98-%EB%A0%88%EB%B2%A8-%EA%B5%AC%ED%95%98%EA%B8%B0) 참고

### 다익스트라 알고리즘으로 최단 거리 구하기

[알고리즘 정리 - 다익스트라 알고리즘](https://github.com/leegwae/algorithms/blob/main/dijkstra-algorithm.md) 참고

### 플로이드 워셜 알고리즘으로 최단 거리 구하기

[알고리즘 정리 - 플로이드 워셜 알고리즘](https://github.com/leegwae/algorithms/blob/main/floyd-warshall-algorithm.md) 참고

### 벨만-포드 알고리즘으로 최단 거리 구하기

[알고리즘 정리 - 벨만-포드 알고리즘](https://github.com/leegwae/algorithms/blob/main/bellman-ford-algorithm.md) 참고
