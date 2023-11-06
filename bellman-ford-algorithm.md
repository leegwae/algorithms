# Bellman-Ford Algorithm

**벨만-포드 알고리즘(Bellman-Ford Algorithm)**은 <u>음수 간선이 있는 그래프</u>에서도 사용할 수 있는 단일 시작점 최단 거리 알고리즘이다.

[다익스트라와 음수 간선이 있는 그래프](https://github.com/leegwae/algorithms/blob/main/dijkstra-algorithm.md#다익스트라와-음수-간선이-있는-그래프)와 같은 경우에서 볼 수 있듯이, 음수 간선이 포함되는 경로에 대해서도 탐색이 필요하다. 따라서 벨만 포드는 그래프의 모든 간선을 탐색하여 최단 경로를 구하고자 한다.



## 벨만-포드 알고리즘의 동작

`W(u, v)`는 `u`에서 `v`로 가는 경로의 가중치를, `D(u)`는 시작 정점 `S`에서 `v`로 가는 최단 거리이다. 처음에는 모든 `D(u)`가 `무한대`로 초기화된다.

1. 초기화하기: 시작 정점이 `S`이므로 `D(S)`를 `0`으로 초기화한다.
2. 완화하기: 모든 간선을 사용하여 완화한다. 간선 `<u, v>`에 대하여, 정점 `v`에 대한 완화는 다음과 같다: `D(u) + W(u, v)`가 `D(v)`보다 작다면 `D(v)`를 `D(u) + W(u, v)`로 갱신한다.
3. 위 과정에서 완화가 발생하지 않을 때까지 반복한다.



## 벨만-포드 알고리즘의 구현

### 초기화하기

```python
dist = [INF] * N
dist[start] = 0
```

- 탐색 전이므로 `dist`의 모든 요소를 `INF(무한대)`로 초기화한다.
- 시작 정점 `start`에 대하여, `dist[start]`는 `0`으로 초기화한다.

### 완화하기

```python
for cur, nxt, cost in edges:
	if dist[cur] != INF and dist[nxt] > dist[cur] + cost:
		dist[nxt] = dist[cur] + cost
```

모든 간선을 사용하여 최단 거리 정보를 갱신한다.

### 전체 코드

```python
# N: 그래프 내 정점의 개수
# edges: 그래프의 간선 리스트
# edges[0] = (출발 정점, 도착 장점, 가중치)
# INF: 양의 무한대
def bellman_ford(start):
	dist = [INF] * N
	dist[start] = 0

	for i in range(N):
		for cur, nxt, cost in edges:
			if dist[cur] != INF and dist[nxt] > dist[cur] + cost:
				dist[nxt] = dist[cur] + cost

				if i == N - 1:
					return []

	return dist
```

[예: 타임머신.py](https://github.com/leegwae/problem-solving/blob/main/graph/bellman-ford/%ED%83%80%EC%9E%84%EB%A8%B8%EC%8B%A0.py)

### 왜 `N-1`번 반복하는가?

모든 간선에 대하여 완화하는 작업을 한 번 반복하면, 경로가 하나의 간선으로 이루어진 최단 거리를 구할 수 있다. 두 번 째에는 경로가 두 개의 간선으로 이루어진 최단 거리를 구할 수 있다. 즉, `n`번째 반복으로 `n`개의 간선으로 이루어진 최단 거리를 구할 수 있다.

한편 정점 `N`개로 이루어진 그래프에서 최단 경로를 이루는 간선의 개수는 `N-1`개이다. 따라서 모든 간선을 완화하는 작업을 `N-1`번 반복하면 최단 경로를 구할 수 있게 된다.

### 왜 `N`번 반복하는가?

그런데 그래프에 **음수 사이클(negative cycle; 사이클을 이루는 간선의 가중치 합이 음수인 사이클)**이 있을 경우, 위 과정을 통해 얻게 된 최단 경로는 올바르지 않다. 아래와 같은 경우를 확인하자.

<img src="https://user-images.githubusercontent.com/57662010/221359632-772553d7-6fb7-4a29-8900-457cac386d70.png" alt="image" width="50%;" />

B-C-D로 이루어진 사이클이 음수 사이클이라고 해보자. 최단 경로는 한 번 방문한 정점은 다시 방문하지 않아야한다. 그러나 벨만-포드는 이미 방문한 정점도 모든 간선을 살펴보며 다시 완화하기 때문에, 모든 간선에 대하여 완화하는 작업을 한 번 반복할 때마다 해당 사이클을 방문하여 비용을 줄인다. 기본적인 벨만-포드 알고리즘이 모든 간선에 대하여 완화하는 작업을 완화가 발생하지 않을 때까지 반복하는 것을 생각해보면, 음수 사이클이 있는 그래프에서는 무한 루프를 돌며 사이클에 포함된 정점은 물론 최단 경로에 해당 정점을 포함하는 정점까지의 비용을 음의 무한대로 계산하게 된다.

따라서 음수 사이클이 없는 그래프일 때는 최대 `N-1`번 루프를 돌아 최단 경로를 구할 수 있으며, 한 번 더 루프를 돌면 음수 사이클이 있는지 확인할 수 있으므로 `N`번 루프를 돌도록 한다.

### 최적화된 벨만 포드

`N`번 모든 간선으로 완화를 반복하는 동안 완화가 일어나지 않는다면 그 이상 루프를 돌 필요가 없으므로 완화가 되었는지 확인하는 플래그를 둘 수 있다.

[bellman-ford.py](https://github.com/leegwae/problem-solving/blob/main/graph/bellman-ford/bellman-ford.py)

```python
def bellman_ford(start):
	dist = [INF] * N
	dist[start] = 0

	for i in range(N):
		updated = False
		for cur, nxt, cost in edges:
			if dist[cur] != INF and dist[nxt] > dist[cur] + cost:
				dist[nxt] = dist[cur] + cost
				updated = True

				if i == N - 1:
					return []
		if not updated:
			break

	return dist
```



## 벨만-포드 알고리즘의 시간 복잡도

정점의 개수를 `V`, 간선의 개수 `E`라고 하자. `N-1`번 모든 간선 `E`개를 순회하므로 `O(VE)`이다.



## 스패닝 트리 구하기

벨만-포드 알고리즘에서 정점을 마지막으로 완화한 간선을 모으면 스패닝 트리를 구할 수 있다. 선택된 간선들은 최단 경로를 구성하고, 최단 경로는 스패닝 트리 위에 있기 때문이다.



## 참고

- 알고리즘 문제해결 전략 30 최단 경로 알고리즘 30.9 벨만-포드의 최단 경로 알고리즘
- [geeksforgeeks - Bellman-Ford Algorithm](https://www.geeksforgeeks.org/bellman-ford-algorithm-dp-23/)



