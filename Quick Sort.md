# Quick Sort

- **퀵 소트(Quick Sort)**: [분할 정복](https://github.com/leegwae/algorithms/blob/main/Divide%20and%20Conquer.md) 기법을 사용하여, 선택한 피벗을 기준으로 배열을 두 개의 비균등한 배열로 나누어 각 부분 배열을 다시 퀵 정렬한다.



## 퀵 정렬의 동작

오름차순을 기준으로 한다.

1. `분할`: 주어진 배열을 `피벗`을 기준으로 두 개의 `부분 배열`로 나눈다.
2. `정복`: 각각의 `부분 배열`을 정렬한다.
3. `병합`: `부분 배열`들을 하나의 배열로 병합한다.



## 퀵 정렬의 구현

### 분할하기

어떻게 `피벗`을 구하여 두 개의 배열로 분할할까?

1. 배열의 마지막 요소를 `피벗`으로 정한다.
2. 배열에서 가장 큰 요소를 찾는다.
3. 배열에서 가장 큰 요소와 배열의 마지막 요소를 교환한다.

```python
def partition(left: int, right: int) -> int:
	pivot = arr[right]
	i = left - 1
	for j in range(left, right):
		if arr[j] <= pivot:
			i = i + 1
			arr[i], arr[j] = arr[j], arr[i]

	arr[i + 1], arr[right] = arr[right], arr[i + 1]

	return i + 1
```



### 정복과 병합

```python
def quick_sort(left: int, right: int):
	if left < right:
		pivot = partition(left, right)
		quick_sort(left, pivot - 1)
		quick_sort(pivot + 1, right)
```



### 전체 코드

[quick_sort.py](https://github.com/leegwae/problem-solving/blob/main/sorting/quick_sort.py)

```python
# arr: 정렬하려는 배열

def partition(left: int, right: int) -> int:
	pivot = arr[right]
	i = left - 1
	for j in range(left, right):
		if arr[j] <= pivot:
			i = i + 1
			arr[i], arr[j] = arr[j], arr[i]

	arr[i + 1], arr[right] = arr[right], arr[i + 1]

	return i + 1

def quick_sort(left: int, right: int):
	if left < right:
		pivot = partition(left, right)
		quick_sort(left, pivot - 1)
		quick_sort(pivot + 1, right)
        

arr = []
quick_sort(0, len(arr) - 1)
print(arr)
```



## 퀵 정렬의 복잡도

### 시간 복잡도

- 최선의 경우와 평균의 경우 `O(nlogn)`이다.
- 최악의 경우는 이미 정렬되어 있는 경우로, `O(n^2)`이다.



### 공간 복잡도

두 원소의 값을 바꾸기 위해 `O(1)`의 공간 복잡도를 필요로 한다.



## 퀵 정렬의 특징

### 장점

- 평균적으로 가장 빠르다.
- 제자리 정렬이다.



### 단점

- 불안정 정렬이다.
- 이미 정렬된 배열에 대해서 비효율적이다.



## 참고

- 자료구조 강의 필기
- [GeeksforGeeks - quick sort](https://www.geeksforgeeks.org/quick-sort/)