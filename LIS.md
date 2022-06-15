# LIS

- **최장 증가 수열(LIS, Longest Increasing Subsequence)**은 수열에서 요소가 증가하는 부분 수열 중 가장 길이기 긴 것을 뜻한다.

```
10 20 10 30 20 50
```

위 수열에서 LIS는 `10 20 30 50`이다.



## LIS의 길이를 구하는 방법

LIS의 길이를 구하는 방법은 세 가지가 있다.

1. 완전 탐색
2. 다이나믹 프로그래밍
3. 이분 탐색



### 완전 탐색으로 LIS의 길이 구하기

완전 탐색은 모든 경우의 수를 탐색하여 답을 구하는 것이다. 이 경우 주어진 수열 `A`에서 각각의 요소 `A[cur]`에 대하여 `A[cur]`이 첫번째 요소인 LIS의 길이를 구하고, 이 중 가장 긴 것을 구하면 된다.

대략적인 구조는 아래와 같게 된다.

```python
def f(A):
    # 수열의 길이가 0이면 LIS의 길이는 0이다.
    if not A:
        return 0

    # A[cur]이 첫번째 요소인 LIS의 최소 길이는 1이다.
    result = 1
    for cur in range(len(A)):
        tmp = A[cur]이 첫번째 요소인 LIS의 길이 구하기
        result = max(result, tmp)
        
    return result
```

`f(A)`는 주어진 수열 `A`에서 LIS의 길이를 반환한다.



#### `A[cur]`이 첫번째 요소인 LIS의 길이 구하기

그렇다면 `A[cur]`이 첫번째 요소인 LIS의 길이는 어떻게 구할 수 있는가? `f(A)`가 주어진 수열 `A`에서 LIS의 길이를 반환하니, `cur` 이후의 수열 `A[cur + 1:]`에 대하여 단순히 `f(A[cur + 1:]) + 1`을 하면 구할 수 있을까?

```python
result = max(result, f(A[cur + 1:]) + 1)
```

그렇지 않다. LIS의 특성상 요소는 오름차순이다. 하지만 부분 수열 `A[cur + 1:]`에는 `A[cur]`보다 작은 요소가 있을 수도 있다. 가령 `A = [4, 2, 1, 5]`에 대하여 부분 수열 `[2, 1, 5]`의 LIS는 `2`이지만 그렇다고 `A`의 LIS가 `3`인건 아니다.

따라서, 새로운 부분 수열 `s`는 `A[cur + 1:]` 중 `A[cur]`보다 큰 요소들을 나열한 부분 수열이어야 한다. 그리고 이 부분 수열 `s`에 대하여 구한 LIS의 길이 `f(s)`에 `1`을 더하면 `A[cur]`이 첫번째 요소인 LIS의 길이를 구할 수 있다.

```python
def f(A):
    if not A:
        return 0
    
    result = 1
	for cur in range(len(A)):
		s = A[cur + 1:]에서 A[cur]보다 큰 요소들만 모은 부분 수열
        result = max(result, 1 + f(s))
        
    return result
```



#### 부분 수열 `s` 구하기

`s`를 구하는 방법은 어렵지 않다. 수열 `A[cur + 1:]`을 `for`문을 돌며 `A[cur]`보다 작은 요소들을 모으면 된다.

```python
# A: 주어진 수열
s = []
for nxt in range(cur + 1, len(A)):
    if arr[cur] < arr[nxt]:
        s.append(arr[nxt])
```



#### 전체 코드

```python
def f(A):
	if len(A) == 0:
		return 0

	result = 1
	for cur in range(0, len(A)):
		s = []
		for nxt in range(cur + 1, len(A)):
			if A[cur] < A[nxt]:
				s.append(A[nxt])
		result = max(result, f(s) + 1)
	return result
```

완전 탐색 풀이의 경우 `O(2^n)`이라는 지수 시간 복잡도를 요구한다.



### 다이나믹 프로그래밍으로 LIS의 길이 구하기

완전 탐색 풀이를 보면 중복 호출이 있다는 것을 알 수 있다. 따라서, 메모이제이션으로 시간 복잡도를 `O(N^2)`으로 줄여볼 수 있다.



### top-down

[출처- LIS의 길이를 구하는 3가지 알고리즘](https://shoark7.github.io/programming/algorithm/3-LIS-algorithms#4b)

```python
# A: 주어진 수열
# N: 수열 A의 길이

def f(cur):
    if dp[cur] != -1:
        return dp[cur]
    
    result = 0
    for nxt in range(cur + 1, N):
        if A[cur] < A[nxt]:
			result = max(result, f(nxt) + 1)
    
    dp[cur] = result
    return result


A = [-math.inf] + A
N = len(A)
dp = [-1] * N
answer = f(0)
print(answer)
```

- `dp[i]`에는 `A[i]`가 첫번째 요소인 LIS의 길이 - 1이 담긴다. 이는 수열 `A`에서 LIS의 길이를 얻기 위해 `f(0)`을 호출하기 이전에, 임의로 `A`에 첫번재 요소로 음의 무한대 `-math.inf`를 삽입했기 때문이다.
- `-math.inf`를 첫번째 요소로 넣은 이유는 아래와 같다.
  - 원본 수열에서 첫번째 요소가 수열에서 가장 큰 요소일 경우 함수 `f()`은 제대로 동작하지 않는다. 가령 수열 `A`가 `[100, 1, 2, 3, 4]`로 이루어져있다고 하자. 기대하는 LIS의 길이는 `4`이나, `f(0)`을 호출하면 `A[0] < A[nxt]`는 모두 `false`를 반환하여 로직이 작동하지 않을 것이기 때문이다.
  - 따라서 수열 첫번째 요소에 임의로 모든 요소보다 작은 값을 넣어 로직이 작동할 수 있도록 한다.
- 인덱스 `cur`에 대해 `A[cur]`과 `A[cur + 1:]`에 있는 요소들을 비교한다.
  - 인덱스 `nxt` (`nxt > cur`)에 대하여 `A[nxt]`가 `A[cur]`보다 크면, `A[nxt]`가 첫번째 요소인 LIS에 `A[cur]`을 첫번째 요소로 삽입한 수열의 길이(즉, `f(nxt) + 1`)와 기존의 값을 비교하여 더 큰 값을 `dp[cur]`에 저장한다.

이 방법은 `O(n^2)`의 시간 복잡도를 요구한다.



### bottom-up

```python
# A: 주어진 수열
# N: 수열 A의 길이

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
- 최초로 `dp[i]`는 `1`로 초기화된다. `A[i]`가 요소로 들어가는 LIS의 최소 길이는 `A[i]`로만 이루어진 수열의 길이 `1`과 같기 때문이다.
- 인덱스 `cur`에 대해 `A[:cur]`에 있는 요소들과 `A[cur]`을 비교한다.
  - 인덱스 `prev` (`prev < cur`)에 대하여 `A[prev]`보다 `A[cur]`이 크면, `A[prev]`가 마지막 요소인 LIS에 `A[cur]`를 더한 수열의 길이(즉, `dp[prev] + 1`)와 기존의 `dp[cur]`의 값을 비교하여 더 큰 값을 `dp[cur]`에 할당한다.

이 방법은 `O(n^2)`의 시간 복잡도를 요구한다.



[예: 가장\_긴\_증가하는\_부분\_수열.py](https://github.com/leegwae/problem-solving/blob/main/LIS/%EA%B0%80%EC%9E%A5_%EA%B8%B4_%EC%A6%9D%EA%B0%80%ED%95%98%EB%8A%94_%EB%B6%80%EB%B6%84_%EC%88%98%EC%97%B4.py)



### 이진 탐색으로 LIS의 길이 구하기

1. `M`은 그 요소가 오름차순으로 유지되는 배열이다.
2. 수열 `A`의 각각의 요소 `A[cur]`에 대하여
   1. `이진 탐색`으로 `M`에 `A[cur]`이 삽입될 위치 `pos`를 구한다.
   2. `pos`가 `M`의 길이와 같다면 `M`에 `A[cur]`를 맨 뒤에 삽입한다.
   3. `pos`와 `M`의 길이와 같지 않다면 `M[pos]`에 `A[cur]`을 덮어씌운다.
      - 이 때문에 `M`은 수열 `A`에 대하여 LIS와 같진 않다.
      - `A = [4, 5, 6, 1, 2]`라고 하자. `for`문이 3번 돈 후에는 `M = [4, 5, 6]`와 같다. `for`문이 끝까지 돌고 나면 `M = [1, 2, 6]`이다. 이처럼 먼저 LIS에 해당하는 부분 수열이 `M`에 저장된 후 다른 값으로 덮어씌워져도 LIS의 길이는 유효하다.

3. `M`의 길이는 수열 `A`에 대하여 LIS의 길이와 같다.

```python
# A: 주어진 수열
# N: 수열 A의 길이

def len_lis() -> int:
	M = []

	for cur in range(N):
		pos = bisect_left(M, A[cur])
		if len(M) == pos:
			M.append(A[cur])
		else:
			M[pos] = A[cur]

	return len(M)

answer = len_lis()
print(answer)
```

이 방법은 `O(NlogN)`의 시간 복잡도를 요구한다. 시간 복잡도가 `O(logN)`인 이진 탐색을 `N`번 수행하기 때문이다.



[예: 가장\_긴\_증가하는_부분\_수열\_2.py](https://github.com/leegwae/problem-solving/blob/main/LIS/%EA%B0%80%EC%9E%A5_%EA%B8%B4_%EC%A6%9D%EA%B0%80%ED%95%98%EB%8A%94_%EB%B6%80%EB%B6%84_%EC%88%98%EC%97%B4_2.py)



## 참고

- [LIS의 길이를 구하는 3가지 알고리즘](https://shoark7.github.io/programming/algorithm/3-LIS-algorithms)
- [최장 증가 부분 수열(LIS) 알고리즘](https://chanhuiseok.github.io/posts/algo-49/)
- [LIS (Longest Increasing Subsequence) - 최장 증가 부분 수열](https://rebro.kr/33)
- [[DP]가장 긴 증가하는 부분 수열(LIS)의 구현 방법](https://juhi.tistory.com/17)
