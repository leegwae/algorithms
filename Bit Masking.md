# Bit Masking

- **비트마스크(Bit Mask)**: 값을 비트 단위로 사용하는 것



## 비트란 무엇인가

- 이진수: 이진법으로 나타낸 수. `0` 또는 `1`이 값을 갖는다.

- **비트(Bit)**: 이진수의 한 자리를 일컫는 말이다. `꺼져있다` 혹은 `켜져있다`고 한다.

- 부호 없는 $N$ 비트 정수형은 $2^0$부터 $2^(N-1)$까지의 값을 표현할 수 있다.

  - 최하위 비트(least significant bit): $2^0$을 나타내는 비트
  - 최상위 비트(most significant bit): $2^{N-1}$을 나타내는 비트

  

## 비트 연산자 알아보기

파이썬의 [비트 연산자](https://github.com/leegwae/python-dojang/blob/main/47.%20Bit%20Operator.md)



## 비트마스크로 집합 구현하기

비트마스크는 집합을 표현할 수 있다.

- `N`비트 정수 변수는 `0`부터 `2^(N-1)`까지의 정수 원소를 가지는 집합을 표현할 수 있다.
- 원소 `i`가 집합에 속한다면, `2^i`를 나타내는 비트가 켜져있다.



### 비트 마스크와 배열

배열은 집합을 표현할 수 있다.

- 길이가 `N`인 집합은 `0`부터 `2^(N-1)`까지의 정수 원소를 가지는 집합을 표현할 수 있다.
- 원소 `i`가 집합에 속한다면, `i`번째 원소는 `True`(혹은 `1`)이다.

따라서, 기존에 배열로 표현했던 것을 비트마스크로도 표현할 수 있다.



### 시작하기

- 여기서는 최하위 비트를 `0`번째 비트로 부르겠다.
- `i`번째 비트가 곧 원소 `i`를 가리킨다고 가정한다. 즉, 원소 `3`을 가졌다면 `100`으로 표현한다.
- `1`을 `N` 왼쪽 쉬프트 연산하는 것은 `1` 뒤에 `0`이 `N`개 붙은 이진수를 만든다. 즉, `N+1`번째 비트만 켜진다.

```python
1 << N
```



### 공집합

상수 `0`이 공집합을 나타낸다.

```python
S = 0
```



### 꽉 찬 집합

`0`부터 `N-1`까지의 모든 수가 집합에 들어가 있다면, `N`비트 정수 변수에서 모든 비트(`N`개)가 켜져있는 것과 같다.

```python
S = (1 << n) - 1
```

```python
N = 3 		# 구하고자 하는 이진수: 111
(1 << 3) - 1 # 7
bin(7)		# '0b111'
```



만약 원소 `x`가 `1`부터 `n`까지 있다면, 편의를 위하여 `n+1`로 초기화해주는 것이 좋다.

```python
S = (1 << (n + 1)) - 1
```



### 원소 추가하기

집합 `S`에 원소 `i`를 추가하는 것은, `N`비트 정수 변수에서 `i`번째 비트를 켜는 것과 같다.

```python
S |= (1 << i)
```



### 원소 삭제하기

집합 `S`에 원소 `i`를 삭제하는 것은, `N`비트 정수 변수에서 `i`번째 비트를 끄는 것과 같다.

```python
S &= ~(1 << i)
```



### 원소가 있는지 확인하기

집합 `S`에 원소 `i`가 있다면, `N`비트 정수 변수에서 `i`번째 비트가 켜져있다는 것이다.

- 연산의 결과가 `0`이라면, `i`번째 비트가 꺼져있다는 것이다.
- 연산의 결과 `1 << i`라면, `i`번째 비트가 켜져있다는 것이다.

```python
S & (1 << i)
```



### 원소 토글하기

집합 `S`에서 원소 `i`를 토글하는 것은 `N`비트 정수 변수에서 `i`번째 비트를 토글하는 것과 같다.

```python
S ^= (1 << i)
```



### 집합 연산하기

```python
a & b	# 교집합
a | b	# 합집합
a & ~b	# 차집합
a ^ b	# a와 b 중 하나에만 포함된 원소들의 집합
```



### 집합의 크기 구하기

```python
def bitCount(x):
    if x == 0: return 0
    return x % 2 + bitCount(x // 2)
```



### 가장 작은 원소 찾기

집합 `S`에서 가장 작은 원소 `i`를 찾는 것은 `N`비트 정수 변수에서 켜져있는 최하위 비트 `i`를 찾는 것과 같다.

```python
B & -B
```

- 음수는 2의 보수를 사용하여 표현한다. 컴퓨터는 음수 `-S`를 구하기 위해 `~S + 1`를 사용한다.
  - `~S`로 켜져있는 최하위 비트 `i`는 꺼지고 그 아래 꺼져있던 비트들은 모두 켜지게된다.
  - `~S`에 `1`을 더하면 `i`는 켜지고 그 아래 비트들은 모두 꺼진다.



### 가장 작은 원소 삭제하기

집합 `S`에서 가장 작은 원소를 삭제하는 것은 `N`비트 정수 변수에서 켜져있는 최하위 비트 `i`를 끄는 것과 같다.

```python
S &= (S - 1)
```

- 켜져있는 최하위 비트가 `i`라면, 그 뒤로는 `i-1`개의 `0`이 있다.
- `S - 1`을 하면 `i`번째 비트는 꺼지고 그 뒤의 비트는 모두 켜지게 된다.
- 따라서 `S & (S - 1)`을 하면 `i`번째 비트는 꺼지고 그 뒤의 비트는 모두 꺼진다.



[예: 집합](https://github.com/leegwae/problem-solving/blob/main/bitmask/%EC%A7%91%ED%95%A9.py)
