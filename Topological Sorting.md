# Topological Sorting

- **위상 정렬(topological sorting)**: 의존성이 있는 작업들이 주어질 때 수행해야 할 순서대로 정렬하는 것이다.



## 위상 정렬과 그래프

각 작업들을 정점으로 표현하고 작업 간의 의존 관계를 간선으로 표현해보자. 이러한 그래프를 **의존성 그래프(dependency grpah)**라고 한다. 의존성 그래프에는 사이클이 없어야 하므로(그래야 정렬할 수 있으므로) 사이클이 없는 방향 그래프, 즉 **DAG(directed acyclic graph)**이다.

따라서 DAG의 정점들을 왼쪽에서 오른쪽으로 모든 정점을 방문할 수 있도록 나열하는 것을 그래프의 관점에서 위상 정렬이라고 할 수 있다. 의존 순서만 만족하면 되므로 유효하게 위상 정렬된 경우의 수는 한 개 이상일 수 있다.



## 위상 정렬의 결과

어떤 **방향 그래프**를 위상 정렬하고자 할 때 다음을 알 수 있다.

1. 그래프에 사이클이 있는가: 그래프에 사이클이 있다면 위상 정렬을 할 수 없다.
2. DAG라면, DAG를 위상 정렬한 결과: DAG의 정점들을 의존 순서대로 나열한 결과를 알 수 있다. 이때 의존 관계를 만족하기만 하면 되므로 유효한 결과의 수는 한 개 이상일 수 있다.



## 위상 정렬의 동작

가장 간단한 위상 정렬은 다음과 같이 동작한다.

1. 진입 차수가 `0`인 정점들을 찾아 하나씩 나열한다.
2. 해당 정점에서 연결된 간선을 모두 지운다. (정점을 그래프에서 지운다.)
3. 모든 정점을 나열할 때까지 위 과정을 반복한다. 모든 정점을 나열할 수 없는 경우, 즉 아직 모든 정점을 나열하지 않았는데 아직 그래프에 간선으로 연결된 정점이 있는 경우 그래프에 사이클이 있으므로 위상 정렬할 수 없다.



## 위상 정렬의 구현

구현 방법에는 여러 가지가 있다. 여기서는 두 방법을 살펴본다.

- 큐를 이용하여 위상 정렬하기
- DFS를 이용하여 위상 정렬하기



여기서는 다음 예시를 사용할 것이다.

<img src="https://user-images.githubusercontent.com/57662010/177457617-418fd646-b407-4497-a8fe-19839af281d3.jpg" alt="DAG" width="40%;" />

```python
# 정점의 개수
N = 6
# graph[v] = [v의 인접 정점의 리스트]
graph = {	
    1: [4],
    2: [3, 5],
    3: [],
    4: [3],
    5: [4],
    6: [2]
}
# in_degree[v] = 정점 v의 진입 차수
in_degree = [0, 0, 1, 2, 2, 1, 0]
```

정점이 `1`부터 시작하므로 편의를 위하여 `in_degree`의 길이는 `N + 1`이다.



## 큐를 이용하여 위상 정렬하기

가장 기본적인 [위상 정렬의 동작](#위상-정렬의-동작)을 구현한다.

1. 진입 차수가 `0`인 정점을 큐에 넣는다.
2. 큐에서 요소를 꺼내어 나열하고 해당 요소의 모든 인접 정점들의 진입 차수를 하나씩 줄인다.
3. 인접 정점의 진입 차수가 `0`이 되면 큐에 넣는다.
4. 큐가 빌 때까지 반복한다.
5. 아직 방문하지 않은 정점이 있는데(아직 나열하지 않은 정점이 있는데) 큐가 비어있다면 사이클이 있으므로 위상 정렬을 진행할 수 없다.



### 전체 코드

[topological_sort.py](https://github.com/leegwae/problem-solving/blob/main/graph/topological_sorting/topological_sort.py#L6)

```python
def topological_sort() -> Optional[List[int]]:
	queue = deque([i for i in range(1, N + 1) if in_degree[i] == 0])
	path = []

	while queue:
		cur = queue.popleft()
		path.append(cur)
		for nxt in graph[cur]:
			in_degree[nxt] -= 1
			if in_degree[nxt] == 0:
				queue.append(nxt)

	return path if len(path) == N else None
```

예시는 `[1, 6, 2, 5, 4, 3]`순으로 방문한다.



## DFS를 이용하여 위상 정렬하기

1. 방문하지 않았다면 해당 정점을 DFS로 탐색한다.
2. 임의의 정점 `v`에서 시작한 DFS가 끝나면 `v`를 나열한다.
3. 모든 정점을 방문할 때까지 반복한다.
4. DFS를 하던 도중 방문한 정점의 인접 정점 `w`가 이미 방문한 적이 있고 나열되어 있지 않다면(즉, `w`를 이미 방문하여 DFS를 시작했는데 `w`에서 시작한 DFS가 아직 끝나지 않았다면) 사이클이 있으므로 위상 정렬을 진행할 수 있다.
5. 사이클이 없어 모든 정점을 방문했다면 나열된 정점을 역순으로 만든다. 탐색이 가장 늦게 끝난 정점일수록 방문이 선행되어있는 정점이기 때문이다.



간단히 하자면, DFS가 끝나는 순서를 역순으로 출력하면 되고 DFS 도중 사이클을 발견하면 출력하지 않으면 된다.



### DFS에서 사이클 발견하기

[사이클과 DFS](https://github.com/leegwae/algorithms/blob/main/DFS.md#%EC%82%AC%EC%9D%B4%ED%81%B4%EA%B3%BC-dfs)를 참고한다.



1. 우선 가장 기본적인 DFS의 구현은 아래와 같다.

```python
N = 정점의 개수
adj = {
    # 인접 리스트로 구현한 그래프
    # 생략
}


def dfs_all():
    visited = [0] * (N)
    
    def dfs(cur):
        visited[cur] = 1

        for nxt in adj[cur]:
            if visited[nxt] == 0:
                dfs(nxt)
                
    for i in range(N):
        if visited[i] == 0:
            dfs(i)
```

`dfs_all()`은 `dfs()`를 여러 번 호출하여 그래프 내의 모든 정점을 방문한다.



2. `dfs_all()` 내부에 `path`라는 배열을 선언하여, 정점 `cur`에서 시작한 DFS가 끝나면 `path`에 `cur`이 들어가도록 해보자.

```python
path = []

def dfs(cur):
    visited[cur] = 1
    
    for nxt in adj[cur]:
        if visited[nxt] == 0:
            dfs(nxt)
            
    path.append(cur)
```

즉, `path`는 `dfs(v)`가 끝난 순서대로 `v`가 들어가있다.



3. 이때 `Line 8`의  `visited[nxt] == 1 and nxt not in path`는 무엇을 의미할까?

```python
def dfs(cur):
    visited[cur] = 1
    
    for nxt in adj[cur]:
        if visited[nxt] == 0:
            dfs(nxt)
        else:
            if nxt not in path:
                print('무엇을 의미할까?')
            
    path.append(cur)
```

먼저 현재 방문 정점은 `cur`이다. 즉, `cur`로부터 DFS를 시작했다. 이때 `cur`의 인접 정점 `nxt`가 있다. `visited[nxt] == 0`이면 아직 `nxt`에는 방문한 적이 없으므로 `dfs(nxt)`를 호출하여 `nxt`로부터 DFS를 시작할 것이다. `visited[nxt] == 1`이라면 `nxt`에 이미 방문했으며 `dfs(nxt)`를 호출하여 `nxt`로부터 DFS를 시작했다는 뜻이다.

한편, `dfs(v)`의 본문 가장 마지막에 `path.append(v)`를 하여 정점 `v`로부터의 DFS를 완료하면 `path`에 `v`를 넣도록 했다. 그렇다면 `nxt in path`란 `nxt`로부터 시작한 DFS가 완료되었다는 뜻이고 `nxt not in path`란 `nxt`로부터 시작한 DFS가 완료되지 않았거나 DFS를 아직 시작하지 않았다는 뜻이다.

따라서 `visited[nxt] == 1`이고 `nxt not in path`는 `nxt`로부터 DFS가 시작했으나 아직 끝나진 않았다는 것을 뜻한다. 

즉 현재 상황은 다음과 같다.  정점 `nxt`에 방문하여 `nxt`에서 DFS를 시작했다. 그래서 탐색을 하던 중 `cur`에 방문하여 `cur`의 인접 정점을 보니 `nxt`가 있었다. `nxt`에서 DFS를 하다보니 다시 `nxt`를 만나게 되었다. 사이클이 있다는 것이다.



4. 그렇다면 사이클이 있는지 나타내는 `has_cycle` 변수를 두어 `visited[nxt] == 1`이고 `nxt not in path`일 때 체크하고 더이상 위상 정렬을 할 수 없으니 탐색을 종료해주면 되겠다.

```python
has_cycle = False

def dfs(cur):
    visited[cur] = 1
    
    for nxt in adj[cur]:
        if visited[nxt] == 0:
            dfs(nxt)
        else:
            if nxt not in path:
                has_cycle = True
                return
            
    path.append(cur)
```



### 전체 코드

[topological_sort.py](https://github.com/leegwae/problem-solving/blob/main/graph/topological_sorting/topological_sort.py#L23)

```python
def dfs_all() -> Optional[List[int]]:
	visited = [0] * (N + 1)
	has_cycle = False
	path = []

	def dfs(cur):
		nonlocal has_cycle

		visited[cur] = 1
		for nxt in graph[cur]:
			if visited[nxt] == 0:
				dfs(nxt)
			elif nxt not in path:
				has_cycle = True
				return

		path.append(cur)

	for i in range(1, N + 1):
		if has_cycle:
			break

		if visited[i] == 0:
			dfs(i)

	return path[::-1] if not has_cycle else None
```

예시에서 정점이 `1`부터 시작하므로 편의를 위하여 `visited`의 길이는 `N + 1`이다. 

예시는 `[6, 2, 5, 1, 4, 3]` 순으로 방문한다.



[예: 음악프로그램.py](https://github.com/leegwae/problem-solving/blob/main/graph/topological_sorting/%EC%9D%8C%EC%95%85%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8.py)
