# Sorting Algorithms

- **정렬 알고리즘(Sorting Algorithm)**은 원소들을 일정한 순서대로 나열하는 알고리즘이다.



## 정렬 알고리즘의 평가 기준

정렬 알고리즘의 평가 기준은 다음과 같다.

1. 비교 횟수가 많은지 적은지
2. 이동 횟수가 많은지 적은지



## 정렬 알고리즘과 관련한 용어

### 안정적인 정렬

- 정렬 알고리즘이 **안정적(stable)**이다: 동일한 키 값을 갖는 요소들의 상대적인 위치가 정렬 후에도 바뀌지 않는 것을 뜻한다.

```
arr = [30, 30, 10, 20]
sorted = [10, 20, 30, 30]
```

가령 `arr[0]`의 30을 `A`, `arr[1]`의 30을 `B`라고 할 때 정렬 후 `A - B` 순이 아니라 `B - A`순으로 불필요하게 위치가 바뀔 경우 안정되지 않은 정렬이다.



### 제자리 정렬

- **제자리 정렬(in-place sorting)**: 정렬하려는 배열 외에는 다른 추가 메모리가 필요하지 않는 정렬을 뜻한다.



## 정렬 알고리즘의 종류

- [선택 정렬(Selection Sort)]((https://github.com/leegwae/algorithms/blob/main/Selection%20Sort.md))
- [삽입 정렬(Insertion Sort)]((https://github.com/leegwae/algorithms/blob/main/Insertion%20Sort.md))
- [버블 정렬(Bubble Sort)]((https://github.com/leegwae/algorithms/blob/main/Bubble%20Sort.md))
- 퀵 정렬(Quick Sort)
- 힙 정렬(Heap Sort)
- [합병 정렬(Merge Sort)]((https://github.com/leegwae/algorithms/blob/main/Merge%20Sort.md))



## 정렬 알고리즘의 복잡도

| 정렬 알고리즘 | 시간 복잡도(최선) | 시간 복잡도(평균) | 시간 복잡도(최악) | 공간 복잡도(최악) |
| ------------- | ----------------- | ----------------- | ----------------- | ----------------- |
| 선택 정렬     | O(n^2)            | O(n^2)            | O(n^2)            | O(1)              |
| 삽입 정렬     | O(n)              | O(n^2)            | O(n^2)            | O(1)              |
| 버블 정렬     | O(n)              | O(n^2)            | O(n^2)            | O(1)              |
| 퀵 정렬       | O(nlogn)          | O(nlogn)          | O(n^2)            | O(logn)           |
| 힙 정렬       | O(nlogn)          | O(nlogn)          | O(nlogn)          | O(1)              |
| 합병 정렬     | O(nlogn)          | O(nlogn)          | O(nlogn)          | O(n)              |

