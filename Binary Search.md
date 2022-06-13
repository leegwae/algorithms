# Binary Search

- **이진 탐색(binary search)**은 분할과 정복 알고리즘을 사용하여 정렬된 배열에서 주어진 요소를 `O(logn)`의 시간복잡도로 검색한다.



## 이진 탐색의 동작

1. 분할: `가운데 요소`를 기준으로 `왼쪽 부분 배열`과 `오른쪽 부분 배열`로 나눈다.                                                                     

2. 정복

   - `가운데 요소`가 `찾는 요소`이면 탐색에 성공했다. 

   - `찾는 요소`가 `가운데 요소`보다 작으면 배열의 범위를 `왼쪽 부분 배열`로 잡는다.

   - `찾는 요소`가 `가운데 요소`보다 크면 배열의 범위를 `오른쪽 부분 배열`로 잡는다.
   - 배열의 길이가 `0`이라면 탐색에 실패했다.

탐색에 성공할 때까지 배열을 둘로 분할하므로 `O(logn)`의 시간복잡도를 요구한다.



## 이진 탐색의 구현

[binary_search.py](https://github.com/leegwae/problem-solving/blob/main/binary_search/binary_search.py)

### 재귀

```python
# N: 찾는 요소
# M: 탐색할 배열
def binary_search(left, right):
	if left > right:
		return -1

	middle = (left + right) // 2

	if M[middle] == N:
		return middle
	elif M[middle] < N:
		return binary_search(middle + 1, right)
	elif M[middle] > N:
		return binary_search(left, middle - 1)

binary_search(0, len(M) - 1)
```



### 반복

```python
# N: 찾는 요소
# M: 탐색할 배열
def binary_search():
	left, right = 0, len(M) - 1
	idx = -1

	while True:
		if left > right:
			break

		middle = (left + right) // 2

		if M[middle] == N:
			idx = middle
			break

		if M[middle] < N:
			left = middle + 1
		elif M[middle] > N:
			right = middle - 1

	return idx
```

