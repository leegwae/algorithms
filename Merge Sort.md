# Merge Sort

- **병합 정렬(Merge Sort)**: [분할 정복 기법](https://github.com/leegwae/algorithms/blob/main/Divide%20and%20Conquer.md)을 사용하여, 정렬하려는 배열을 두 개의 배열로 나눈 후 다시 합치며 정렬하는 알고리즘이다.



## 병합 정렬의 동작

오름차순을 기준으로 한다.

1. `분할`: 주어진 배열을 두 개의 `부분 배열`로 나눈다.
2. `정복`: 각각의 `부분 배열`을 정렬한다.
3. `병합`: `부분 배열`들을 하나의 배열로 병합한다.



## 병합 정렬의 구현

[merge_sort.py](https://github.com/leegwae/problem-solving/blob/main/sorting/merge_sort.py)

```python
# arr: 정렬하려는 배열
# N: arr의 길이
def merge_sort(left: int, right: int) -> None:
	if left >= right:
		return

	middle = (left + right) // 2
	merge_sort(left, middle)
	merge_sort(middle + 1, right)
	merge(left, middle, right)


def merge(left: int, middle: int, right: int) -> None:
	i = left
	j = middle + 1
	k = left

	while i <= middle and j <= right:
		if arr[i] <= arr[j]:
			sorted[k] = arr[i]
			k += 1
			i += 1
		else:
			sorted[k] = arr[j]
			k += 1
			j += 1

	while i <= middle:
		sorted[k] = arr[i]
		i += 1
		k += 1

	while j <= right:
		sorted[k] = arr[j]
		j += 1
		k += 1

	for i in range(left, right + 1):
		arr[i] = sorted[i]
        

merge_sort(0, N - 1)
```



## 합병 정렬의 복잡도

### 시간 복잡도

시간 복잡도는 모든 경우에서 `O(nlogn)`이다.



### 공간 복잡도

정렬하려는 배열 크기 만큼의 배열을 사용해야하므로 공간 복잡도는 `O(n)`이다.



## 합병 정렬의 특징

### 장점

- 안정 정렬이다.
- 크기가 큰 연결 리스트를 합병 정렬할 때, 연결하는 링크만 변경하면 되므로 매우 효율적이다.



### 단점

- 제자리 정렬이 아니다: 배열 크기만큼의 메모리를 필요로 한다.
- 배열의 크기가 크다면 이동 횟수와 필요로 하는 메모리가 커져 비효율적이다.
