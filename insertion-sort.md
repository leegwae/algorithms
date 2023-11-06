# Insertion Sort

- **삽입 정렬(Insertion Sort)**: 배열의 원소를 차례대로 그 앞의 원소들과 비교하여, 정렬 기준에 따라 삽입하는 정렬 알고리즘이다.



## 삽입 정렬의 동작

오름차순을 기준으로 한다.

1. 현재 위치(`i >= 1`)의 요소를 저장한다.
2. 현재 위치의 이전 요소들을 역순으로 검사하여 저장한 값보다 크다면 한 칸 씩 뒤로 옮긴다.
3. 저장한 값을 빈 칸에 삽입한다.



## 삽입 정렬의 구현

[insertion_sort.py](https://github.com/leegwae/problem-solving/blob/main/sorting/insertion_sort.py)

```python
def insertion_sort(arr):
    N = len(arr)
    for i in range(1, N):
        val = arr[i]
        prev = i - 1
        while prev >= 0 and arr[prev] > val:
            arr[prev + 1] = arr[prev]
            prev -= 1
        arr[prev + 1] = val
```

`arr[prev]=val`가 아니라 `arr[prev+1]=val`인 이유는 `val`보다 큰 마지막 요소를 뒤로 당겨 해당 칸이 비게 되었는데 `prev-=1`을 하여 그 앞 칸을 가리키게 되었기 때문이다.



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