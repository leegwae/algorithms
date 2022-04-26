# etc

## split() 사용하여 여러 개의 숫자 입력받기

```python
numbers = list(map(int, input().split()))
```



## input() 대신 sys.stdin.readline() 사용하기

- `input()`은 `sys.stdin.readline()`과 다음에서 차이를 가진다.

1. 인자로 prompt message를 전달받는다.
2. 전달받은 prompt message를 출력한 후 입력을 받는다.
3. 받은 입력값에 개행 문자`\n`를 제거한 후 반환한다.

이 때문에 `input()`은 `sys.stdin.readline()`보다 느리기도 하다. 따라서 사용자로부터 입력값을 받기 위해 `input()` 대신 `sys.stdin.readline()`을 사용한다.

```python
import sys

n = sys.stdin.readline()
```



- ``sys.stdin.readline()`으로 받은 입력값에는 개행 문자가 붙어있으므로, `strip()`을 이용하여 제거할 수 있다.

```python
import sys
n = sys.stdin.readline().strip()
```



- 한편, `sys.stdin.readline()`이 `input()`에 비해 길어 부담된다면 다음과 같이 써보자.

```python
import sys

realine = sys.stdin.readline
```



## queue 대신 deque 사용하기

- 자료구조 `큐`의 `pop()` 연산은 파이썬의 리스트 `pop(0)` 연산으로 구현된다.

```python
queue = [1, 2, 3, 4, 5]
item = queue.pop(0)
```

이 `pop(k)` 연산은 `O(N)`의 시간복잡도를 가진다. 

- 큐의 양쪽에서 삽입, 삭제 연산을 지원하는 자료구조 `collections.deque`의 `popleft()` 연산은 `O(1)`의 시간복잡도를 가진다.

```python
from collections import deque
queue = deque([1, 2, 3, 4, 5])
item = queue.popleft()
```

따라서 `큐`의 연산을 사용할 일이 있다면 `collections.deque`를 사용하도록 한다.



[E10. Deque](https://github.com/leegwae/python-dojang/blob/main/E10.%20Deque.md) 참고

