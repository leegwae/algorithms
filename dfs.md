# DFS

- **깊이 우선 탐색(DFS; depth-first search)**: 한 방향으로 갈 수 있을 때까지 가다가 더 이상 갈 수 없게 되면 가장 가까운 갈림길로 돌아와 다시 탐색하는 것
- 되돌아가기 위해 주로 `스택`을 사용하여 구현한다.



## DFS의 원리

1. 먼저 정점 `v`에서 시작한다. `v`에 방문하였음을 표시한다.
2. 정점 `v`의 인접 정점들을 `for`문으로 돈다. 인접 정점 `w`가 아직 방문하기 전이라면, 해당 정점으로 이동한다.
3. `1번`에서 계속

```
DFS(G, v)
	v에 방문하였다고 표시
	for w in v의 인접 정점들:
		if w에 방문하지 않았다면:
			DFS(G, w)
```



<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbFzHDv%2FbtrgCFGxcoc%2FkCSFISCVblHbvKf1esSRZK%2Fimg.jpg" alt="graph" width="50%;" />

DFS로는 `0-> 1-> 3-> 6-> 4-> 2-> 5` 혹은 `0->2->5->1->4->3->6`순으로 탐색할 것이다.



## DFS의 구현

- DFS는 `재귀`를 사용하여 구현하거나, `스택`을 사용하여 `반복`으로 구현할 수 있다.



### 준비

1. 우선 위 그림의 `graph`를 인접 리스트로 구현한다.

```python
graph = {
    0: [1, 2],
    1: [0, 3, 4],
    2: [0, 5],
    3: [1, 6],
    4: [1],
    5: [2],
    6: [3]
}
```

2. 정점들을 방문했는지 여부를 나타낼 수 있는 배열 `visited`를 만든다.

```python
visited = [0] * (정점의개수)
```



#### visited는 어떻게 선언하는가

한편, `visited`는 두가지 방법으로 구현해볼 수 있다.

1. 요소가 `방문한 정점`을 나타냄

```python
visited = []		# visited 배열 선언하기
visited.append(w)	# 방문한 정점 표시하기
if w in visited:	# 정점에 방문했는지 알아보기
    print(f'{w}에 이미 방문하였음')
```

- 요소에 방문할 때마다 `visited`에 `방문한 정점`을 요소로 추가한다. 
- 이 방법은 이전에 해당 정점을 방문했는지 알기 위해 `in` 연산자를 사용하므로 `O(n)`의 시간복잡도를 가진다.
- 정점을 순회한 순서대로 리스트에 요소로 추가되므로, `visited`를 출력하면 DFS로 순회한 결과를 얻을 수 있다.

2. 인덱스가 `방문한 정점`을 나타내고 요소는 해당 정점을 `방문했는지`를 나타냄

```python
visited = [0] * (정점의개수)		# visited 배열 선언하기
visited[w] = 1					# 방문한 정점 표시하기
if visited[w] == 1:				# 정점에 방문했는지 알아보기
    print(f'{w}에 이미 방문하였음')
```

- 요소에 방문할 때마다 `visited[w]`를 `1`로 플래그한다.
- 이 방법은 이전에 해당 정점을 방문했는지 알기 위해 인덱스를 사용하여 리스트의 요소에 접근하므로 `O(1)`의 시간복잡도를 가진다.



따라서 상황에 따라 `visited`를 구현하도록 한다. 여기서는 `2번. 인덱스로 방문한 정점 나타내기`를 사용한다.



### 재귀로 구현하기

```python
graph = {...}
visited = [0] * 7

def dfs(cur):
    visited[cur] = 1
    # print(cur)

    for nxt in graph[cur]:
        if visited[nxt] == 0:
            dfs(nxt)

dfs(0)
```

- `[0, 1, 3, 6, 4, 2, 5]`순으로 방문할 것이다.



### 스택으로 구현하기

```python
graph = {...}
visited = [0] * 7

def dfs(start):
    stack = [start]
    visited[start] = 1
    
    while stack:
        cur = stack.pop()
        # print(cur)
        
        for nxt in graph[cur]:
            if visited[nxt] == 0:
                visited[nxt] = 1
                stack.append(nxt)
```

- `[0, 2, 5, 1, 4, 3, 6]`순으로 방문할 것이다.



### 재귀 구현과 스택 구현의 차이점

재귀 구현과 스택 구현의 출력 결과가 다른 것은 잘못된 것이 아니다.

- `스택`의 `pop()` 연산은 리스트에서 가장 마지막 요소를 반환한다. 정점 `v`에 대하여 인접 정점들의 리스트인 `graph[v]`의 요소들은 인덱스 순서대로 리스트에 들어갔다가 (현재 정점에서는 더 이상 탐색할 정점이 없으면) 역순으로 나오게 된다.
- 한편, 재귀를 이용한 구현에서 `graph[v]`의 요소들은 인덱스 순서대로 접근되어 해당 정점에서 탐색을 시작한다.

위 그림을 예시로 들자면 `0->1`로 가느냐(재귀 구현) `0->2`로 가느냐(스택 구현)의 차이가 있겠다.



### 언제 `visited[w]`를 확인할까?

DFS는 `재귀`와 `스택(반복)` 구현 외에도 인접 정점 `w`의 방문 여부를 나타내는 `visited[w]`를 확인하는 시점으로 구현 방법을 구분해볼 수 있다.

1. 확인하고 호출하기: 방문하지 않았다면 `dfs(w)`를 호출하기
2. 호출하고 확인하기: `dfs(w)`를 호출하고 방문했다면 리턴하기

문제에 따라 `1번`을 적용하는 것이 편할 수도, `2번`을 적용하는 것이 편할 수도 있다. 위 [재귀로 구현하기](https://github.com/leegwae/algorithms/blob/main/DFS.md#%EC%9E%AC%EA%B7%80%EB%A1%9C-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0)와 [스택으로 구현하기](https://github.com/leegwae/algorithms/blob/main/DFS.md#%EC%8A%A4%ED%83%9D%EC%9C%BC%EB%A1%9C-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0)는 `1번`의 방법을 따랐다. 아래는 `2번`으로 구현한 DFS이다.



#### 재귀로 구현하기

```python
def dfs(cur):
    if visited[cur] == 1:
        return
    
    visited[cur] = 1
    # print(cur)

    for nxt in graph[cur]:
        dfs(nxt)
```



#### 스택으로 구현하기

```python
def dfs(start):
	stack = [start]

	while stack:
		cur = stack.pop()

		if visited[cur] == 0:
			visited[cur] = 1
			print(cur)

			for nxt in graph[cur]:
				stack.append(nxt)
```



## DFS의 시간복잡도

정점의 개수 `V`, 간선의 개수 `E`

- `인접 리스트`로 그래프를 구현한 경우: `O(V+E)`
  - 한 번 방문한 정점은 다시 방문하지 않는다. 
  - 하나의 정점에서 다른 정점을 방문하는 횟수가 해당 정점의 차수가 같기 때문이다.
- `인접 행렬`로 그래프를 구현한 경우: `O(V^2)`
  - `dfs(v)`는 `v`에 대하여 그래프의 다른 모든 정점이 인접하는지 알기 위해 `graph[v]`를 `for`문을 사용하여 순회한다. 따라서 `O(V)`의 시간복잡도를 가진다.
  - 그래프의 모든 정점에 대하여 `dfs(v)`를 호출하므로 `O(V*V)=O(V^2)`의 시간복잡도를 가진다.



## 비연결 그래프에서 DFS

### 정말 모든 정점을 순회할까?

앞에서는 모든 정점 쌍에 대하여 경로가 존재하는 `연결 그래프`를 예시로 함수들을 구현했다. 이 함수들로는 `비연결 그래프`의 모든 정점들을 순회할 수는 없다. `dfs(시작정점)`으로 호출할 때 `시작정점`에서 갈 수 있는 정점들만, 즉 `시작정점`이 속한 `컴포넌트`만 방문할 것이기 때문이다.

<img src="https://user-images.githubusercontent.com/57662010/164989596-c6badc3f-1a80-4c9b-ad35-820a76d8cf5d.jpg" alt="connected component" width="40%;" />

가령, `dfs(1)`을 호출한다면 `1->3->2`순으로 정점들을 방문할 뿐 정점 `[1, 2, 3]`에 인접하지 않은 정점 `[4, 5]`로는 갈 수 없다.



### 컴포넌트의 개수 세기

`비연결 그래프`에서 모든 정점을 순회한다는 것은 모든 `컴포넌트`를 방문해야한다는 것과 같다. 모든 컴포넌트를 방문하려면 임의의 컴포넌트 하나를 일단 순회한 후 그래프에 남은 정점이 있는지, 즉 아직 순회하지 않은 컴포넌트가 있는지 확인하고 해당 컴포넌트를 방문하면 될 것이다.

1. `dfs(v)`를 호출한다.
2. 호출이 끝나면 `visited`로 아직 방문하지 않은 정점 `w`가 있는지 살펴본다.
3. `1번`에서 계속



```python
# N: 그래프 내 정점의 개수
N = 5
graph = {
    1: [3, 2],
    2: [1],
    3: [1],
    4: [5],
    5: [4]
}

visited = [0] * (N + 1)
count = 0


def dfs(v):
    visited[v] = 1

    for w in graph[v]:
        if visited[w] == 0:
            dfs(w)

for v in range(1, N + 1):
    if visited[v] == 0:
        dfs(v)
        count += 1

print(count)
```

- `2`가 출력될 것이다.



[예: 유기농 배추](https://github.com/leegwae/problem-solving/blob/main/dfs/%EC%9C%A0%EA%B8%B0%EB%86%8D_%EB%B0%B0%EC%B6%94_2.py)



## 사이클이 있는 그래프에서 DFS

<img src="https://user-images.githubusercontent.com/57662010/165721791-a7fa8853-d7e7-49f6-9689-c605542ce7df.jpg" alt="cycle graph" width="40%" />

위 그래프는 `순환 그래프`로, `2, 3, 4`가 사이클에 속한다. 주어진 정점 `v`가 사이클에 속하는지 알려면 `visited[v] == 1`만으로는 충분하지 않다. 

정점 `v`의 인접 정점에서 DFS를 모두 마치고 돌아왔을 때 `finished[v] = 1`로 표시한다고 하자. 즉 `finished[v] == 1`이란 `v`에서부터 시작한 DFS 탐색(`dfs(v)`)이 끝이 났다는 것을 뜻한다.

```python
graph = {
    1: [2],
    2: [3],
    3: [4],
    4: [2]
}

def dfs(v):
	visited[v] = 1

	for w in graph[v]:
		if visited[w] == 0:
			dfs(w)
            
    finished[v] = 1
```

이때, `visited[w] == 1` 이면서 `finished[w] == 0`은 무슨 뜻일까? 

```python
def dfs(v):
	visited[v] = 1

	for w in graph[v]:
		if visited[w] == 0:
			dfs(w)
		else:
			if finished[w] == 0:
				print(f'정점 {w}는 이미 방문했으나, 정점 {w}부터 시작한 DFS가 아직 끝이 나지 않았음')
            
    finished[v] = 1
```

그것은 `w`를 방문하고 DFS를 진행하던 중 방문한 정점 `v`의 인접 정점이 `w`였다는 뜻이다. 곧, `w`와 `v`는 사이클을 이루고 있다.



### 사이클에 속하는 요소 알아보기

> 주의: 이 방법은 모든 정점이 단 하나의 인접 정점만을 가질 때 유효하다.

방문한 정점이 인덱스가 아니라 요소가 되도록 `visited`의 구현 방식을 바꾸어야한다.

```python
visited = []

def dfs(v):
	visited.append(v)

    w = graph[v]
    if w not in visited:
        dfs(w)
    else:
        if finished[w] == 0:
            print(visited[visited.index(w):])

	finished[v] = 1
```

`visited`에 방문한 정점을 차례대로 넣는다고 하자. 현재 방문 정점이 `v`이고 `v`의 인접 정점이 `w`인데 `w in visited`이고 `finished[w] == 0`라면 `v`와 `w`는 사이클을 이루고 있다. 그렇다면 `w`에서 `v`까지 오는 동안 지나왔던 정점들은 모두 사이클에 속한다. `visited`의 상태는 `[..., w, ..., v]`이므로, `visited[visited.index(w):]`로 해당 사이클에 속하는 요소를 모두 알 수 있다.



[예: 텀 프로젝트](https://github.com/leegwae/problem-solving/blob/main/graph/dfs/%ED%85%80_%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8.py)



## 사이클이 없는 그래프에서 DFS

### 위상 정렬

**위상 정렬(topological sorting)**이란 의존 관계가 있는 정점들을 순서에 맞게 나열하는 것을 의미한다. 이것은 DAG(directed acylic graph)를 DFS하며 DFS가 끝난 순서대로 정점을 나열한 것과 같다.



[알고리즘 위상 정렬 정리](https://github.com/leegwae/algorithms/blob/main/sorting-algorithms.md) 참고



## 트리의 지름

트리의 지름은 트리에서 가장 멀리 떨어져있는 두 정점의 거리이다. 이 거리는 두 정점 사이의 간선의 개수와 동일하다. 트리의 지름은 다음과 같이 구할 수 있다.

1. 트리에서 임의의 정점 A를 고르고, A에서 가장 멀리 떨어진 정점 B를 구한다.
2. B에서 가장 멀리 떨어진 정점 C를 구한다. 이때 B와 C 사이의 거리가 트리의 지름이다.

트리에서 임의의 두 정점 사이의 거리는 DFS나 BFS로 구할 수 있다.

```python
B, _ = dfs(A)
C, tree_diameter = dfs(B)
```

[예: 트리의_지름.py](https://github.com/leegwae/problem-solving/blob/main/graph/dfs/%ED%8A%B8%EB%A6%AC%EC%9D%98_%EC%A7%80%EB%A6%84.py)

[예: 스크루지_민호.py](https://github.com/leegwae/problem-solving/blob/main/graph/bfs/%EC%8A%A4%ED%81%AC%EB%A3%A8%EC%A7%80_%EB%AF%BC%ED%98%B8.py)
