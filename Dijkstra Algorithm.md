# Dijkstra Algorithm

- **다익스트라 알고리즘(Dijkstra Algorithm)**은
  - 단일 시작점 최단 경로 알고리즘이다: 시작 정점에서부터 다른 정점들까지의 최단 거리를 구한다.
  - 그리디 알고리즘에 속한다: 항상 노드 주변의 최단 경로만을 택한다.
  - 노드 주변을 탐색할 때 BFS를 사용한다.



## 다익스트라 알고리즘의 동작

`W(u, v)`는 `u`에서 `v`로 가는 경로의 가중치를, `D(u)`는 시작 정점 `S`에서 `v`로 가는 최단 거리이다. 처음에는 모든 `D(u)`가 `무한대`로 초기화된다.

1. [초기화하기](#초기화하기): 시작 정점이 `S`이므로 `D(S)`를 `0`으로 초기화한다.
2. [방문할 정점 구하기](#방문할-정점-구하기): 아직 방문하지 않은 정점 중 `D(u)`가 가장 짧은 `u`를 선택하여 방문한다. (최초로는 `D(S)`가 `0`으로 가장 작으므로 `S`를 방문하게 된다.)
3. [완화하기](#완화하기): 현재 방문한 정점 `u`의 인접 정점 중, 아직 방문하지 않은 정점 `v`에 대하여 `D(u) + W(u, v)`가 `D(v)`보다 작다면 `D(v)`를 `D(u) + W(u, v)`로 갱신한다. (인접 정점의 최단 거리 정보를 갱신하는 것을 `완화한다`고 한다.)
4. 방문할 정점이 없을 때까지 `2번`부터 반복한다.



## 다익스트라 알고리즘의 구현

```
초기화하기
방문할 정점이 없을 때까지
	방문할 정점 구하기
	완화하기(인접 정점의 최단 거리 정보 갱신하기)
```



### 초기화하기

```python
visited = [0] * N
distances = [INF] * N

distances[start] = 0
```

- `visited`는 정점의 방문 정보를 담는다.
- 탐색 전이므로 `distances`의 모든 요소를 `INF(무한대)`로 초기화한다.
- 시작 정점 `start`에 대하여, `distances[start]`는 `0`으로 초기화한다.



### 방문할 정점 구하기

```python
def get_closest():
    min_distance = INF
    min_pos = -1

    for i in range(N):
        if visited[i] == 0:
            if distances[i] < min_distance:
                min_pos = i
                min_distance = distances[i]
	
    return min_pos

closest = get_closest()
visited[closest] = 1
```

- `get_closest()`는 아직 방문하지 않은 정점 중 `distances[i]`가 가장 짧은 `i`를 선택한다. 더이상 방문할 정점이 없다면 `-1`을 반환한다.
- `get_closest()`로 구한 현재 방문 정점 `closest`에 방문했음을 표시한다.



### 완화하기

```python
for adj in range(N):
    if graph[closest][adj] == INF or visited[adj] == 1:
        continue
	distances[adj] = min(distances[adj], distances[closest] + graph[closest][adj])
```

- 현재 방문 정점 `closest`의 인접 정점 중, 방문하지 않은 정점 `adj`의 최단 거리 `distances[adj]`를 갱신한다.



### 전체 코드

[dijkstra.py](https://github.com/leegwae/problem-solving/blob/main/graph/dijkstra/dijkstra.py)

```python
# N: 그래프 내 정점의 개수
# graph: 인접 행렬로 구현
# graph[v][w] = weight
# INF: 양의 무한대
def dijkstra(start: int) -> List[int]:
	visited = [0] * N
	distances = [INF] * N

	def get_closest():
		min_distance = INF
		min_pos = -1

		for i in range(N):
			if visited[i] == 0:
				if distances[i] < min_distance:
					min_pos = i
					min_distance = distances[i]

		return min_pos

	distances[start] = 0

	while True:
		closest = get_closest()

		if closest == -1:
			break

		visited[closest] = 1
		for adj in range(N):
			if graph[closest][adj] == INF or visited[adj] == 1:
				continue

			distances[adj] = min(distances[adj], distances[closest] + graph[closest][adj])

	return distances
```

`25-26`: `get_closest()`는 더이상 방문할 정점이 없을 때 `-1`을 반환하므로, 이때 루프를 멈추도록 한다.



## 다익스트라 알고리즘과 BFS

다음과 같은 그래프가 있다고 하자.

<img src="https://user-images.githubusercontent.com/57662010/175524973-abe0c168-2179-4caf-bc1b-1de8204260e2.jpg" alt="graph" width="70%;" />

시작 정점 `S`에서 `B`로 가는 최단 경로는 `S - A - C - B`가 될 것이다. 그러나 BFS로 `S`부터 탐색한다면 `A`와 `B`를 먼저 방문한 후 `C`에 방문하게 된다. 다익스트라는 **우선순위 큐**를 사용하여 이 문제를 해결한다.



## 우선순위 큐로 동작하는 다익스트라 알고리즘

우선순위 큐는 **최소 힙**으로, 정점 `v`에 대하여 `최단 경로 D(v)`를 기준으로 정렬된다.

1. [초기화하기](#초기화하기-2):
   1. 시작 정점이 `S`이므로 `D(S)`를 `0`으로 초기화한다.
   2. 우선순위 큐 `PQ`에 `(D(S), S)`를 넣는다.
2. [방문할 정점 구하기](#방문할-정점-구하기-2): `PQ`에서 `(D(u), u)`를 꺼낸다. (최초에 `PQ`에는 요소가 `(D(S), S)` 밖에 없으므로, `S`에 방문하게 된다.)
3. [완화하기](#완화화기-2): 현재 방문한 정점 `u`의 인접 정점 중, 아직 방문하지 않은 정점 `v`에 대하여 `D(u) + W(u, v)`가 `D(v)`보다 작다면
   1. `D(v)`를 `D(u) + W(u, v)`로 갱신한다.
   2. `PQ`에 `(D(u), u)`를 넣는다.
4. `PQ`가 빌 때까지 `2번`부터 반복한다.



## 우선순위 큐로 구현한 다익스트라 알고리즘

### 초기화하기

```python
distances = [INF] * N

distances[start] = 0
queue: List[Tuple[int, int]] = [(distances[start], start)]
```

- 탐색 전이므로 `distances`의 모든 요소를 `INF(무한대)`로 초기화한다.
- 시작 정점 `start`에 대하여, `distances[start]`는 `0`으로 초기화한다.
- 우선순위 큐에 `(D(start), start)`를 넣는다.



### 방문할 정점 구하기

```python
cur_dist, cur = heappop(queue)
```

- 우선순위 큐에서 `cur`과 현재까지 계산한 `cur`까지의 최단 경로 `cur_dist`를 꺼낸다.



### 완화하기

```python
for nxt, weight in graph[cur].items():
    nxt_dist = cur_dist + weight

    if nxt_dist < distances[nxt]:
        distances[nxt] = nxt_dist
        heappush(queue, (nxt_dist, nxt))
```

1. 현재 방문 정점 `cur`의 인접 정점 `nxt`에 대하여, `cur`부터 `nxt`까지의 가중치 `weight`를 구한다.
2. `cur_dist + weight`가 `distance[nxt]`보다 작다면 `distance[nxt]`를 갱신한다. 그리고 우선순위 큐에 `(distance[nxt], nxt)`를 넣는다.



### 전체 코드

[예: 최단경로.py](https://github.com/leegwae/problem-solving/blob/main/graph/dijkstra/최단경로.py)

```python
# N: 그래프 내 정점의 개수
# INF: 양의 무한대
# graph: 인접 리스트로 구현
# graph = {}
# graph[u] = { v: 가중치 }

def dijkstra(start: int) -> List[int]:
	distances = [INF] * N

	distances[start] = 0
	queue: List[Tuple[int, int]] = [(distances[start], start)]

	while queue:
		cur_dist, cur = heapq.heappop(queue)

		if distances[cur] < cur_dist:
			continue

		for nxt, weight in graph[cur].items():
			nxt_dist = cur_dist + weight

			if nxt_dist < distances[nxt]:
				distances[nxt] = nxt_dist
				heapq.heappush(queue, (nxt_dist, nxt))

	return distances
```



## 다익스트라 알고리즘의 시간 복잡도

정점의 개수를 `V`, 간선의 개수 `E`라고 하자.



### 우선순위 큐를 사용하지 않은 경우

1. 모든 정점은 한 번씩 방문되므로 모든 간선은 한 번씩 검사된다. 그러므로 `O(E)`
2. 각각의 정점에 대하여 인접 정점이 어떤 정점인지 검사한다. 그러므로 `O(V^2)`
3. 따라서 우선순위 큐를 사용하지 않은 경우 `O(V^2 + E)`



### 우선순위 큐를 사용한 경우

1. 모든 정점은 한 번씩 방문되므로 모든 간선은 한 번씩 검사된다. 그러므로 `O(E)`
2. 최악의 경우 우선순위 큐에는 `O(E)`만큼 원소가 추가된다. 우선순위 큐에 삽입/삭제하는 연산은 `O(logE)`이다. 그러므로 `O(E) * O(logE) = O(ElogE)`
3. 전체 시간 복잡도는 `O(E)` + `O(ElogE)` = `O(ElogE)`이다.
4. 이때 `E`는 `V^2`보다 작으므로, `O(logE) = O(logV)`라고 할 수 있다.
5. 따라서 우선순위 큐를 사용하지 않은 경우 `O(E * logV)`



## 언제 우선순위 큐를 사용하는 것이 효율적인가?

시간 복잡도에 따라

- 정점의 개수가 많다면 우선순위 큐를 사용한다.
- 간선의 개수가 많다면 우선순위 큐를 사용하지 않는다.



##  다익스트라와 음수 간선이 있는 그래프

다익스트라를 사용한다고 해서 음수 간선이 있는 그래프에 대해 모든 경우 최단 경로를 구하지 못하는 것은 아니다. 다만 항상 최단 경로를 구할 수 있다고 보장하지 못한다.

 아래와 같은 경우를 살펴보자.

<img src="https://user-images.githubusercontent.com/57662010/221352887-9ed5152e-41f1-462b-bc67-e3b9afd15934.png" alt="image" width="50%;" />

B에서 C로 가는 최단 경로를 구하려고한다. B에서 C로 가면 `5`만큼 들고, B에서 A를 경로하여 C로 가면 `7 + -5 = 2`만큼 드므로 B-A-C가 B에서 C로 가는 최단 경로이다. 그러나 탐욕적 속성을 가진 다익스트라를 사용하면, B에서 먼저 방문할 정점을 선택할 때 가중치가 더 낮은 간선 B-C를 선택하게 된다. 따라서 정점 C에 완화가 완료되어 더이상 정점 C에 대한 최단 거리 정보는 갱신되지 않을 것이다.

그러니 음수 간선이 있는 그래프에 최단 경로를 구하고자 한다면 벨만 포드 알고리즘을 사용하도록 한다. [벨만 포드 알고리즘 정리](https://github.com/leegwae/algorithms/blob/main/Bellman-Ford%20Algorithm.md)를 참고한다.
