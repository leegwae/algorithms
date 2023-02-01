# Minimum Spanning Tree

- **스패닝 트리(Spanning Tree; 신장 트리)**는 무방향 그래프의 모든 정점과 간선의 일부로 구성된 트리이다.
- **최소 스패닝 트리(MST, Minimum Spanning Tree)**는 주어진 무방향 그래프의 스패닝 트리 중 가중치의 합이 가장 작은 트리이다. (즉, 사이클이 없으며, 정점의 개수가 `N`개라면 간선의 개수는 `N-1`이고 연결 그래프이다.)



## 최소 스패닝 트리 알고리즘의 종류

최소 스패닝 트리를 구하는 알고리즘은 대표적으로 다음과 같다.

1. 크루스칼 알고리즘(Kruskal's Algorithm)
2. 프림 알고리즘(Prims's Algorithm)

두 알고리즘 모두 **탐욕법**을 기반으로 하여, 트리에 간선을 하나씩 추가하며 최소 스패닝 트리를 구한다.



## 크루스칼 알고리즘

### 크루스칼 알고리즘의 동작

1. 간선을 가중치의 오름차순으로 정렬한다.
2. 간선을 순서대로 하나씩 트리에 더한다. 이때, 트리에 사이클이 생긴다면 제외한다.
3. 트리의 간선의 개수가 `N-1`개가 되면 MST이다.



### 크루스칼 알고리즘의 구현

1. DFS/BFS 사용하기: 간선을 트리에 더할 때마다 그래프를 순회하여 사이클이 있는지 확인한다.
2. [Union-Find](https://github.com/leegwae/data-structures/blob/main/Union-Find.md) 사용하기: 간선을 이루는 두 정점이 같은 컴포넌트에 속하는지 확인한다. (같은 컴포넌트에 속한다면 사이클이 있다.)

#### union-find 사용하기

[kruskal.py](https://github.com/leegwae/problem-solving/blob/main/minimum_spanning_tree/kruskal.py)

```python
def kruskal():
	edges = []
	edges.sort()
    
	selected = []
	total = 0
	for cost, a, b in edges:
		if find(a) == find(b):
			continue
	
		union(a, b)
		selected.append((a, b))
		total += cost
	return selected, total
```

이때 입력 그래프가 연결 그래프가 아니라면, 크루스칼 알고리즘의 결과로 선택된 간선이 이루는 그래프 역시 연결 그래프가 아니다. (간선의 개수는 `N-1`이 아니다.)

[도시_건설.py](https://github.com/leegwae/problem-solving/blob/main/minimum_spanning_tree/%EB%8F%84%EC%8B%9C_%EA%B1%B4%EC%84%A4.py)



### 크루스칼 알고리즘의 복잡도

간선의 개수가 `V`, 정점의 개수가 `E`이다.

#### 시간 복잡도

1. DFS/BFS를 사용한 경우, 간선마다 그래프 순회하므로 `E * O(E + V) = O(E^2)`
2. union-find를 사용한 경우, 간선 정렬에 걸리는 시간이 가장 크므로 `O(ElogE)`



## 참고

- https://blog.naver.com/kks227/220799105543
- 구종만-알고리즘 문제 해결 전략 31장 최소 스패닝 트리
