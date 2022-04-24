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

