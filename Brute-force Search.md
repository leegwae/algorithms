# Brute-force Search

- **완전 탐색(brute-force search)**: 가능한 경우를 모두 탐색하는 방법



## 브루트 포스의 원리

모든 가능한 경우를 탐색하여 문제를 해결하므로, 대개 `for문`을 사용한다.



### 예: 연구소

[문제](https://www.acmicpc.net/problem/14502)는 다음과 같이 요약할 수 있다.

- 크기가 `N*M`인 연구소가 있다.
- 연구소는 빈 칸(`0`), 벽(`1`), 바이러스가 들어있는 칸(`1`)으로 이루어져있다.
- 바이러스는 위, 아래, 왼쪽, 오른쪽의 빈 칸으로 퍼질 수 있다.
- 벽 3개를 세울 수 있다고 할 때, 확보할 수 있는 안전 영역의 최대 크기를 구하라.



다음과 같이 해결할 수 있다.

1. <u>`for문`을 사용하여 벽 3개를 세운 모든 경우</u>를 구하고, 그래프에 벽 3개를 세운다.
   1. 벽 3개를 세운 그래프에 대하여, DFS로 바이러스가 퍼지도록 한다.
   2. 바이러스가 퍼진 그래프에 대하여, 안전 영역의 크기를 구한다.
2. 최대 안전 영역의 크기를 구한다.

[연구소.py](https://github.com/leegwae/problem-solving/blob/main/bruteforcing/%EC%97%B0%EA%B5%AC%EC%86%8C.py)

```python
max_count = 0
for i in range(zero_len - 2):
	for j in range(i + 1, zero_len - 1):
		for k in range(j + 1, zero_len):
			copied = copy.deepcopy(graph)
			for idx in [i, j, k]:
				y, x = zero_list[idx]
				copied[y][x] = 1

			for n in range(N):
				for m in range(M):
					if graph[n][m] == 2:
						dfs(n, m)
			max_count = max(max_count, get_safe_count(copied))
print(max_count)
```

