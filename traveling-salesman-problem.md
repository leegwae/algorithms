# Traveling Salesman problem

**외판원 순회(Traveling Salesman problem, TSP)**는 출발 정점에서부터 시작하여 모든 정점을 중복 없이 방문한 다음 출발 정점으로 돌아오는 경로 중 최소의 비용을 찾는 문제이다.



`N`개의 정점과 가중치의 행렬(`graph[i][j]`는 `i`에서 `j`로 가는 비용, `0`이면 간선이 없다)이 주어질 때, 외판원 순회에 필요한 최소 비용을 구해보자.

가장 먼저 떠오르는 방법은 백트래킹을 통한 완전 탐색이다. 백트래킹을 사용하여 정점을 나열할 수 있는 모든 경우의 수(순열)를 구하고 이 중에서 정점 `i`에서 `j`로 갈 수 없는 경우를 제한 다음, 순회에 필요한 비용을 구한 후 그 중에서 최소 비용을 찾는 것이다.

백트래킹을 사용하여 `0`부터 `N-1`까지의 수를 나열하는 모든 경우의 수를 찾아보자.

```python
def backtracking(visited):
    if len(visited) == N:
        print(visited)

    for i in range(N):
        if i not in visited:
            backtracking([*visited, i])
```

외판원 순회는 `0`부터 `N-1`까지의 수를 나열한 후 다시 `0`을 나열해야하며, `i` 뒤에 `j`를 나열할 수 있으려면 가중치가 `0` 이상이어야한다. 그러니 `i not in visited`로는 충분하지 않으며 `graph[i][j] > 0`도 검사해서 경우의 수를 만들어주면 되겠다.

```python
import sys

input = sys.stdin.readline


def dfs(visited, cur, total):
	global answer
	if len(visited) == N and graph[cur][0] > 0:
		answer = min(answer, total + graph[cur][0])
		return

	for nxt in range(N):
		if nxt not in visited:
			if graph[cur][nxt] > 0:
				dfs([*visited, nxt], nxt, total + graph[cur][nxt])


if __name__ == '__main__':
	N = int(input())
	graph = [list(map(int, input().split())) for _ in range(N)]
	answer = int(1e09)
	dfs([0], 0, 0)
	print(answer)
```

하지만 이렇게 하면 중복되는 호출이 많다. 예를 들어, 1부터 7까지 나열하는 두 가지 경로가 있다고 하자.

```
1 2 3 4 5 6 7
1 3 2 4 5 6 7
```

첫번째 경로로 가는 비용을 구했다. 그런데 두번째 경로와 첫번째 경로가 겹치는 부분이 있다. `4 5 6 7`이다. 두 경로는 다르지만 `4 5 6 7`을 건너는 비용은 당연히 동일하다. 굳이 `4 5 6 7`을 건너가며 다시 계산하지 않아도 이 비용을 어딘가에 저장해놓으면 다시 써먹을 수 있지 않을까?

또한 `dfs(visited, cur)`을 호출할 때, `cur`로 시작하고 `visited`에 속하지 않는 정점들로 나열된 경로의 최소 비용을 얻을 수 있다고 생각한다면 중복되는 부분 문제들을 풀 수 있어보인다. 즉 DP로 바꾸어 생각해보자.

하지만 `visited`는 배열이라 DP 배열의 인덱스로 적절하지 않아 보인다. 그렇다면 `visited`를 인덱스로 사용할 수 있는 값으로 바꾸는 것이 적절할 것이다. 비트마스킹을 사용해보자.

```python
1 << N;	// 텅 빈 집합
(1 << N) -1;	// 꽉 찬 집합
S & (1 << i);	// 0이 아니면 S에 i가 켜져있다.
S | (1 << i);	// S에 i를 켜기
```



```python
INF = int(1e09)
N = int(input())
graph = []
dp = [[-1] * (1 << N) for _ in range(N)]


def dfs(cur, visited):
	if visited == (1 << N) - 1:
		return graph[cur][0] if graph[cur][0] > 0 else INF

	if dp[cur][visited] != -1:
		return dp[cur][visited]

	result = INF
	for nxt in range(N):
		if not (visited & (1 << nxt)) and graph[cur][nxt] > 0:
			result = min(result, dfs(nxt, visited | (1 << nxt)) + graph[cur][nxt])

	dp[cur][visited] = result
	return dp[cur][visited]

print(dfs(0, 1 << 0))
```



