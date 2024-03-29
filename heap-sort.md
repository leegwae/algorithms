# Heap Sort

- **힙 정렬(Heap Sort)**은 [자료구조 힙](https://github.com/leegwae/data-structures/blob/main/heap.md)를 사용하여 데이터를 정렬하는 정렬 알고리즘이다.



## 힙 정렬의 동작

1. 힙 생성 알고리즘을 사용하여 탐색 배열을 힙으로 만든다.
2. 힙에서 요소를 꺼내어 차례대로 나열한다.
3. 탐색 배열의 길이가 1이 될 때까지 반복한다.

내림차순으로 정렬하려면 최대 힙, 오름차순으로 정렬하려면 최소 힙을 사용한다.

> 힙은 요소를 꺼낼 때 루트에 있는 요소를 꺼내는데, 이 요소가 최대 힙에서는 최댓값, 최소 힙에서는 최솟값이다. 따라서 요소를 꺼낸 순대로 나열한다면 최대 힙을 사용할 경우 내림차순으로 정렬되고 최소 힙을 사용한다면 오름차순으로 정렬된다.

이 글에서는 최대 힙을 사용한다.

### 힙 생성 알고리즘

**힙 생성 알고리즘(Heapify Algorithm)**은 주어진 완전 이진 트리가 힙의 성질을 만족하도록 요소의 위치를 바꾸는 알고리즘이다. 이때, 완전 이진 트리에서 **하나의 노드만** 힙의 성질을 만족하지 않는 것으로 가정한다. 

하향식 힙 생성 알고리즘은 타겟 노드의 값을 자식 노드의 값과 비교한 뒤 타겟 노드의 값이 작다면 자식 노드와 자리를 바꾼다. 자리를 바꾼 값을 다시 해당 자리에서의 자식 노드의 값과 비교한다. 마찬가지로 값이 작다면 자식 노드와 자리를 바꾼다. 이 과정을 자식 노드가 존재하지 않을 때까지 반복한다.

상향식 힙 생성 알고리즘은 타겟 노드의 값을 부모 노드의 값과 비교한 뒤 타겟 노드의 값이 크다면 부모 노드와 자리를 바꾼다. 자리를 바꾼 값을 다시 해당 자리에서의 부모 노드의 값과 비교한다. 마찬가지로 값이 크다면 부모 노드와 자리를 바꾼다. 이 과정을 루트 노드에 도달할 때까지 반복한다.

이 글에서는 하향식 힙 생성 알고리즘을 사용한다.

#### 힙 생성 알고리즘의 구현

```python
def heapify(arr: List[int], size, idx):
	largest = idx
	left = 2 * idx + 1
	right = 2 * idx + 2

	if left < size and arr[largest] < arr[left]:
		largest = left

	if right < size and arr[largest] < arr[right]:
		largest = right

	if largest != idx:
		arr[idx], arr[largest] = arr[largest], arr[idx]
		heapify(arr, size, largest)
```

자식 노드의 값이 부모 노드의 값보다 크다면 두 노드의 값을 바꾸는데, 두 자식 노드의 값이 모두 부모 노드의 값보다 크다면 두 자식 노드 중 더 큰 노드의 값이 선택된다. 그래야지 노드의 값을 바꾸었을 때 힙의 성질을 유지할 수 있기 때문이다.

#### 힙 생성 알고리즘의 복잡도

- 최악의 경우 루트가 마지막 레벨까지 내려가야한다. 이때 비교 연산과 이동 연산의 횟수는 트리의 높이만큼 이루어지므로 시간 복잡도는 `O(logn)`이다. 
- 두 원소의 값을 바꾸기 위해 `O(1)`의 공간 복잡도를 필요로 한다.



## 힙 정렬의 구현

힙 정렬은 두 가지 부분으로 나눌 수 있다.

1. 주어진 배열을 최대 힙으로 만든다.
2. 최대 힙을 사용하여 오름차순으로 정렬된 배열을 만든다.

### 1. 최대 힙 만들기

```python
N = len(arr)
for i in range(N // 2 - 1, -1, -1):
    heapify(arr, N, i)
```

모든 요소에 대하여 heapify를 돌 필요는 없다. 자식 노드가 있는 노드에 대해서만 heapify를 돌면 된다. 즉, 자식 노드가 있는 노드 중 가장 마지막 노드(가장 아래에 있고 가장 오른쪽에 있는 노드)부터 루트 노드까지 heapify를 돌면 된다. 이 자식 노드가 있는 가장 마지막 노드의 인덱스는 `N // 2 - 1`이고, 루트 노드의 인덱스는 `0`이므로 위와 같이 루프를 도는 것이다.

그런데, 루프를 돌지 않고 `heapify(arr, N, 0)`만 실행해도 되지 않을까? 루트부터 heapify를 돌기 때문에 자연스럽게 자식 노드도 heapify를 돌 수 있지 않냐는 거다. 결론부터 적자면 그렇지 않다. 루트 노드와 루트 노드의 두 자식 노드가 최대 힙의 성질 - *"부모 노드의 값이 자식 노드의 값보다 크다"*를 만족한다면 `if largest != idx`를 만족하지 않아 재귀를 돌지 않을 것이기 때문이다. 루트 노드와 루트 노드의 두 자식 노드만이 최대 힙의 성질을 만족하지 않는다면 재귀를 돌게 되므로 괜찮겠지만, 루트 노드와 루트 노드의 두 자식 노드가 최대 힙의 성질을 만족하는데 트리 내부에 어떤 부모 노드와 그 두 자식 노드가 최대 힙의 성질을 만족하지 않는다면 해당 노드까지 재귀가 돌지 않을 것이므로 반드시 `N // 2 - 1`번의 노드부터 루프를 돌도록 한다.

### 2. 최대 힙으로 오름차순 정렬 배열 만들기

[힙 정렬의 동작](#힙-정렬의-동작)에서는 *내림차순으로 정렬하려면 최대 힙, 오름차순으로 정렬하려면 최소 힙을 사용한다*고 적었다. 하지만 추가적인 메모리 사용 없이 원본 배열에 요소를 정렬하여 저장하고 싶다면 내림차순의 경우 최소 힙, 오름차순의 경우 최대 힙을 사용해야 한다.

```python
for i in range(N - 1, 0, -1):
    arr[i], arr[0] = arr[0], arr[i]
    heapify(arr, i, 0)
```

루트 노드를 트리에서 제거하는 대신 마지막 노드와 바꾼다. 이때 마지막 노드는 배열에서 정렬되지 않은 부분에서 가장 마지막 노드이다. 이후 배열에서 정렬되지 않은 부분에 대해서만 heapify를 돌리고 배열에서 정렬되지 않은 부분의 길이가 0이 될 때까지 계속한다.

### 전체 코드

[heap_sort.py](https://github.com/leegwae/problem-solving/blob/main/sorting/heap_sort.py)

```python
def heapify(arr: List[int], size, idx):
	largest = idx
	left = 2 * idx + 1
	right = 2 * idx + 2

	if left < size and arr[largest] < arr[left]:
		largest = left

	if right < size and arr[largest] < arr[right]:
		largest = right

	if largest != idx:
		arr[idx], arr[largest] = arr[largest], arr[idx]
		heapify(arr, size, largest)


def heap_sort(arr: List[int]):
	N = len(arr)
	for i in range(N // 2 - 1, -1, -1):
		heapify(arr, N, i)
	for i in range(N - 1, 0, -1):
		arr[i], arr[0] = arr[0], arr[i]
		heapify(arr, i, 0)
```



## 힙 정렬의 복잡도

### 시간 복잡도

heapify를 N번 실행하므로 모든 경우에서 `O(nlogn)`이다.

### 공간 복잡도

두 원소의 값을 바꾸기 위해 `O(1)`의 공간 복잡도를 필요로 한다.



## 힙 정렬의 특징

- 장점
  - 효율적이다.
  - 제자리 정렬이다.
- 단점
  - 불안정 정렬이다.
  - 탐색 배열을 힙으로 만드는데 많은 시간이 들 수 있다.