# Insertion Sort

- **삽입 정렬(Insertion Sort)**: 배열의 원소를 차례대로 그 앞의 원소들과 비교하여, 정렬 기준에 따라 삽입하는 정렬 알고리즘이다.



## 삽입 정렬의 동작

오름차순을 기준으로 한다.

1. 현재 인덱스 `i` (` i >= 1`)에 대하여 `arr[i]`를 저장한다.
2. 저장한 값을 인덱스 `i` 이전의 요소들 `arr[p]` (`p < i`)과 비교하여, 저장한 값이 더 작다면 `arr[p]`를 한칸씩 뒤로 당긴다.
3. 저장한 값이 크다면 `arr[p + 1]`에 할당한다.
4. 현재 인덱스 `i`가 `n`이 될 때까지 반복



## 삽입 정렬의 구현

[insertion_sort.py](https://github.com/leegwae/problem-solving/blob/main/sorting/insertion_sort.py)

```python
# arr: 정렬하려는 배열
# N: arr의 길이
def insertion_sort():
	for i in range(1, N):
		key = arr[i]
		prev = i - 1
		while prev >= 0 and arr[prev] > key:
			arr[prev + 1] = arr[prev]
			prev -= 1
		arr[prev + 1] = key
```



## 삽입 정렬의 복잡도

### 시간 복잡도

- 최선의 경우는 `O(n)`으로 이미 정렬되어 있는 경우이다.
  - 비교 횟수: `n - 1`번
- 최악의 경우는 `O(n^2)`으로 역순으로 정렬되어 있는 경우이다.
  - 비교 횟수: `O(n^2)`
  - 이동 횟수: `O(n^2)`
- 평균의 경우는 `O(n^2)`



### 공간 복잡도

두 원소의 값을 바꾸기 위해 `O(1)`의 공간 복잡도를 필요로 한다.



## 삽입 정렬의 특징

- 장점

  - 안정 정렬이다.

  - 제자리 정렬이다.

  - 대부분 정렬되어있으면 매우 효율적이다.

- 단점

  - 이동 횟수가 많다.
  - 최악과 평균의 경우 비효율적이다.