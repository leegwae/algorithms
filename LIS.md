# LIS

- **최장 증가 수열(LIS, Longest Increasing Subsequence)**은 수열에서 요소가 증가하는 부분 수열 중 가장 길이기 긴 것을 뜻한다.

```
10 20 10 30 20 50
```

위 수열에서 LIS는 `10 20 30 50`이다.



## LIS를 구하는 방법

LIS를 구하는 방법은 두 가지가 있다.

1. 다이나믹 프로그래밍
2. 이분 탐색



### 다이나믹 프로그래밍으로 LIS의 길이 구하기

```python
# N: 수열의 길이
# A: 주어진 수열

def f(n):
	for cur in range(1, n):
		for prev in range(0, cur):
			if A[cur] > A[prev]:
				dp[cur] = max(dp[cur], dp[prev] + 1)

	return max(dp)

dp = [1] * N
answer = f(N)
print(answer)
```

- `dp[i]`에는 `A[i]`가 마지막 요소인 LIS의 길이가 담긴다.
- 인덱스 `cur`에 대해 `A[:cur]`에 있는 요소들과 `A[cur]`을 비교한다.
  - `A[prev]`(`prev < cur`)보다 `A[cur]`이 크면, `A[prev]`가 마지막 요소인 LIS에 `A[cur]`의 길이(즉, `dp[prev] + 1`)와 기존의 `dp[i]`의 값을 비교하여 더 큰 값을 `dp[i]`에 할당한다.

이 방법은 `O(n^2)`의 시간 복잡도를 요구한다.



[예: 가장\_긴\_증가하는\_부분\_수열.py](https://github.com/leegwae/problem-solving/blob/main/LIS/%EA%B0%80%EC%9E%A5_%EA%B8%B4_%EC%A6%9D%EA%B0%80%ED%95%98%EB%8A%94_%EB%B6%80%EB%B6%84_%EC%88%98%EC%97%B4.py)



## 참고

- https://shoark7.github.io/programming/algorithm/3-LIS-algorithms
- https://chanhuiseok.github.io/posts/algo-49/
- https://rebro.kr/33
- https://juhi.tistory.com/17
