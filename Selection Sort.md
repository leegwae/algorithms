# Selection Sort

- **선택 정렬(Selection Sort)**: 현재 차례에서 정해진 위치에 어떤 원소를 넣을지 선택하는 정렬 알고리즘이다.



## 선택 정렬의 동작

오름차순을 기준으로 한다.

1. 탐색 배열에서 최소값을 찾는다.
2. 탐색 배열의 첫번째 요소와 찾은 최소값을 바꾼다.
3. 탐색 배열에서 첫번째 요소를 제외한다.
4. 탐색 배열의 길이가 `1`이 될 때까지 반복한다.



## 선택 정렬의 구현

[selection_sort.py](https://github.com/leegwae/problem-solving/blob/main/sorting/selection_sort.py)

```python
def selection_sort(arr):
    N = len(arr)
    for i in range(0, N - 1):
        least = i
        for j in range(i + 1, N):
            if arr[j] > least:
                least = j
        arr[i], arr[least] = arr[least], arr[i]
```

동일한 값을 갖는 요소가 여러 개일 때, 가장 마지막 요소가 먼저 선택되어 정렬되므로 불안정 정렬이다.



## 선택 정렬의 복잡도

### 시간 복잡도

비교 횟수가 `(n - 1) + (n - 2) + ... + 1 = n(n-1) / 2`이므로 시간 복잡도는 `O(n^2)`이다.

### 공간 복잡도

두 원소의 값을 바꾸기 위해 `O(1)`의 공간 복잡도를 필요로 한다.



## 선택 정렬의 특징

- 장점
  - 단순하다.
  - 제자리 정렬이다.
- 단점
  - 비효율적이다.
  - 불안정 정렬이다.

