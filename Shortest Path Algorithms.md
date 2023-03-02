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

### BFS로 최단 거리 구하기

[BFS.md - 노드의 레벨 구하기 & 두 노드 사이의 최단거리 구하기](https://github.com/leegwae/algorithms/blob/main/BFS.md#%EB%85%B8%EB%93%9C%EC%9D%98-%EB%A0%88%EB%B2%A8-%EA%B5%AC%ED%95%98%EA%B8%B0) 참고

### 다익스트라 알고리즘으로 최단 거리 구하기

[알고리즘 정리 - 다익스트라 알고리즘](https://github.com/leegwae/algorithms/blob/main/Dijkstra%20Algorithm.md) 참고

### 플로이드 워셜 알고리즘으로 최단 거리 구하기

[알고리즘 정리 - 플로이드 워셜 알고리즘](https://github.com/leegwae/algorithms/blob/main/Floyd-Warshall%20Algorithm.md) 참고

### 벨만-포드 알고리즘으로 최단 거리 구하기

[알고리즘 정리 - 벨만-포드 알고리즘](https://github.com/leegwae/algorithms/blob/main/Bellman-Ford%20Algorithm.md) 참고
