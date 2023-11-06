# Binary Search

- **이진 탐색(binary search)**은 분할과 정복을 사용하여 정렬된 배열에서 탐색 범위를 절반으로 좁히며 주어진 요소를 탐색하는 알고리즘이다.



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

	while left <= right:
		middle = (left + right) // 2

		if M[middle] == N:
            return middle
        
		if M[middle] < N:
			left = middle + 1
		elif M[middle] > N:
			right = middle - 1

	return -1
```



## 파이썬과 이진 탐색

파이썬의 `bisect` 모듈이 이진 탐색을 지원한다.

| 메서드                        | 시간 복잡도 | 설명                                                         |
| ----------------------------- | ----------- | ------------------------------------------------------------ |
| `bisect_left(리스트, 아이템)` | O(logn)     | 리스트에 아이템을 삽입할 때 삽입할 수 있는 가장 왼쪽 인덱스를 반환한다. |

```python
import bisect
from typing import List

def binary_search(arr: List[int], target: int) -> int:
    idx = bisect.bisect_left(arr, target)
    
    if idx < len(arr) and arr[idx] == target:
        return idx
    else:
        return -1
```



## 이진 탐색과 오버플로 문제

```python
middle = (left + right) // 2
```

자료형의 최댓값이 있는 언어에서,`left + right`가 이 최대값을 넘어선다면 예상치 못한 결과나 오류를 초래할 수 있다.

```python
middle = left + (right - left) // 2
```

두 수의 합에서 반을 구하는 대신, 두 수의 차를 반으로 나눈 값을 작은 수에 더하면 오버플로를 피할 수 있다. `right - left`는 항상 `right` 보다 작기 때문에 오버플로를 피할 수 있고, `left`에 `right - left // 2`를 더한 값도 항상 `right`보다 작기 때문에 오버플로를 피할 수 있다.



## 이진 탐색의 복잡도

### 시간 복잡도

최악의 경우 N개의 요소를 1개의 요소가 될 때까지 범위를 좁혀야한다. 이때 범위를 줄이는 연산이 logn번 반복되므로 O(logn)의 시간 복잡도를 필요로 한다.

예) 탐색 범위 8개 -> 4개 -> 2개 -> 1개로 줄일 때까지 줄이는 횟수 $log_2{8}$=3번 수행되었다.



## 이진 탐색으로 풀 수 있는 문제들

- 최댓값 중 최솟값, 최솟값 중 최댓값을 찾는 문제들

