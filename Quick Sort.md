# Quick Sort

- **퀵 소트(Quick Sort)**: [분할 정복](https://github.com/leegwae/algorithms/blob/main/Divide%20and%20Conquer.md) 기법을 사용하여, 선택한 피벗을 기준으로 배열을 두 개의 비균등한 배열로 나누어 각 부분 배열을 다시 퀵 정렬한다.



## 퀵 정렬의 동작

오름차순을 기준으로 한다.

1. `분할`: 주어진 배열을 `피벗`을 기준으로 두 개의 `부분 배열`로 나눈다.
2. `정복`: 각각의 `부분 배열`을 정렬한다.
3. `병합`: `부분 배열`들을 하나의 배열로 병합한다.

```python
def quick_sort(left, right):
	if left < right:
		pivot_idx = partition(left, right)
		quick_sort(left, pivot_idx - 1)
		quick_sort(pivot_idx + 1, right)
```



## 퀵 정렬의 구현

퀵 정렬에서 **분할과 정렬**을 구현하는 방법은 여러 가지이다.

1. 피벗 선택하기: 가장 왼쪽의 요소, 가장 오른쪽의 요소, 혹은 가운데 요소를 피벗으로 선택한다.
2. 요소 옮기기: 기본적인 아이디어는 피벗을 기준으로 왼쪽에 피벗보다 작은 요소들을, 오른쪽에 피벗보다 큰 요소들을 모으는 것이다. 두 가지 방법이 있다.
   1. 가장 왼쪽의 요소가 피벗으로 선택되었다면 피벗보다 큰 요소를 배열의 오른쪽에 모은다. 가장 오른쪽의 요소가 피벗으로 선택되었다면 피벗보다 작은 요소를 배열의 왼쪽에 모은다. 마지막으로 피벗을 기준으로 나누어지도록 피벗을 옮긴다.
   2. 피벗을 기준으로 탐색하지 않은 요소 중 가장 작은 요소와 큰 요소를 택하여 swap한다. 마지막으로 피벗을 기준으로 나누어지도록 피벗을 옮긴다.

### 첫번째 방법

1. 배열의 마지막 요소를 `피벗`으로 정한다.
2. `피벗`을 제외한 요소들 중 `피벗` 보다 작은 요소는 왼쪽으로 모은다.
3. `피벗`을 `피벗`보다 작은 요소들 옆으로 옮겨 `피벗`을 기준으로 작은 요소들과 큰 요소들로 나누어지도록 한다.

```python
def partition(left, right):
	pivot = arr[right]
	pos = left
	for i in range(left, right):
		if arr[i] <= pivot:
			arr[pos], arr[i] = arr[i], arr[pos]
			pos = pos + 1

	arr[pos], arr[right] = arr[right], arr[pos]
	return pos
```

왼쪽부터 차례대로 검사하며 피벗보다 작은 요소는 swap을 사용하여 왼쪽으로 모은다. `pos`는 피벗이 들어갈 인덱스를 나타내게 된다.

#### 전체 코드

[quick_sort.py](https://github.com/leegwae/problem-solving/blob/main/sorting/quick_sort.py)

```python
def partition(left, right):
	pivot = arr[right]
	pos = left
	for i in range(left, right):
		if arr[i] <= pivot:
			arr[pos], arr[i] = arr[i], arr[pos]
			pos = pos + 1

	arr[pos], arr[right] = arr[right], arr[pos]
	return pos


def quick_sort(left, right):
	if left < right:
		pivot_idx = partition(left, right)
		quick_sort(left, pivot_idx - 1)
		quick_sort(pivot_idx + 1, right)
```

### 두번째 방법

1. 배열의 첫번째 요소를 피벗으로 정한다.
2. 피벗을 제외한 요소들 중에서 가장 작은 요소와 가장 큰 요소를 찾고, 가장 큰 요소의 인덱스가 가장 작은 요소의 인덱스보다 작다면 둘의 위치를 바꾼다.
3. 탐색이 끝났다면 피벗을 옮긴다.

```python
def partition(left, right):
	low = left + 1
	high = right
	pivot = arr[left]

	while low <= high:
		while low <= right and arr[low] <= pivot:
			low += 1
		while high >= left and arr[high] > pivot:
			high -= 1

		if low <= high:
			arr[low], arr[high] = arr[high], arr[low]

	arr[left], arr[high] = arr[high], arr[left]
	return high
```

#### 전체 코드

[quick_sort.py](https://github.com/leegwae/problem-solving/blob/main/sorting/quick_sort.py)

```python
def partition(left, right):
	low = left + 1
	high = right
	pivot = arr[left]

	while low <= high:
		while low <= right and arr[low] <= pivot:
			low += 1
		while high >= low and arr[high] > pivot:
			high -= 1

		if low <= high:
			arr[low], arr[high] = arr[high], arr[low]

	arr[left], arr[high] = arr[high], arr[left]
	return high


def quick_sort(left, right):
	if left < right:
		pivot_idx = partition(left, right)
		quick_sort(left, pivot_idx - 1)
		quick_sort(pivot_idx + 1, right)

```





## 퀵 정렬의 복잡도

### 시간 복잡도

- 최선의 경우와 평균의 경우 `O(nlogn)`이다.
- 최악의 경우는 `O(n^2)`이다. pivot이 최댓값이나 최솟값으로 선정되면 `n`개의 요소는 `1`개와 `n-1`개로 쪼개지고, `n-1`개도 이 과정을 반복하다보면 재귀 호출은 총 `n`번 이루어진다. 각 재귀 호출에서 요소를 비교하는 연산이 `n`번 이루어지므로 O(n*2)이다.

### 공간 복잡도

두 원소의 값을 바꾸기 위해 `O(1)`의 공간 복잡도를 필요로 한다.



## 퀵 정렬의 특징

- 장점

  - 평균적으로 가장 빠르다.

  - 제자리 정렬이다.

- 단점

  - 불안정 정렬이다.

  - 이미 정렬된 배열에 대해서 비효율적이다.




## 참고

- 자료구조 강의 필기
- [GeeksforGeeks - quick sort](https://www.geeksforgeeks.org/quick-sort/)