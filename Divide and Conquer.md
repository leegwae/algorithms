# Divide and Conquer

- **분할 정복(divide and conquer)**: 다음과 같이 세 부분으로 나눌 수 있다.
  1. `분할`: 문제를 두 가지 이상의 부분 문제로 분할한다.
  2. `정복`: 가장 간단한(작은) 하위 문제(기저 사례)를 푼다.
  3. `조합`: 하위 문제들의 답을 병합하여 원래 문제의 답을 푼다.



## 분할 정복의 동작

분할 정복은 문제를 더작은 부분 문제로 나누며, `재귀`를 사용한다.

```python
def f(x):
    if f(x)가 간단하면:
        return f(x)를 계산한 값
    
    x를 x1, x2로 분할한다.
    f(x1);
    f(x2);
    return f(x1)과 f(x2)의 값으로 계산한 f(x)의 값
```



## 분할 정복을 적용할 수 있는 문제들

분할 정복을 적용하여 풀이할 수 있는 문제들은 아래와 같다.

1. **최적 부분 구조(Optimal Substructure)**을 가진 문제



### 최적 부분 구조

문제의 가장 좋은 해결 방법이 부분 문제들의 가장 좋은 해결 방법으로 구성된 것을 뜻한다.

[다이나믹 프로그래밍 - 최적 부분 구조](https://github.com/leegwae/algorithms/blob/main/Dynamic%20Programming.md#%EC%B5%9C%EC%A0%81-%EB%B6%80%EB%B6%84-%EA%B5%AC%EC%A1%B0) 참고



## 분할 정복으로 풀 수 있는 문제들

- 퀵 정렬



### 예: 병합 정렬

[병합 정렬](https://github.com/leegwae/algorithms/blob/main/Divide%20and%20Conquer.md) 참고



### 예: 이진 탐색

[이진 탐색](https://github.com/leegwae/algorithms/blob/main/Binary%20Search.md) 참고



### 예: 거듭제곱

> 주어진 수 `x`의 `n`제곱을 구하라.

일반적으로 `반복`을 이용하면 시간 복잡도는 `O(n)`이다.

```python
def power(x, n):
    result = 1
    
    for _ in range(n):
        result *= x
        
    return result
```



`분할 정복`을 이용하면 시간 복잡도는 `O(logN)`이다.

1. `분할`하기: 문제를 하위 문제로 나누어본다. 가령 `n`이 짝수라면, `x^n = x^(n/2) * x^(n/2)`이다.

```
power(x, n) = {
	power(x, n/2) * power(x, n/2), (n이 짝수)
	power(x, n/2) * power(x, n/2) * x, (n이 홀수)
}
```

2. `정복`하기: 가장 간단한 문제, 곧 `기저 사례`를 찾는다. `n`이 `0`이라면, `x`의 값에 관계없이 `x^n = 1`이다.

```python
if n == 0:
    return 1
```

3. `조합`하기: 하위 문제를 병합한다.

```python
def power(x, n):
    if n == 0:
        return 1
    
    temp = power(x, int(n / 2))
    if n % 2 == 0:
        return temp * temp
    else:
        return x * temp * temp
```



### 예: 쿼드 트리

> `N`*`N` 크기의 영상 `M`이 주어진다. 각 칸은 `0` 혹은 `1`로 채워져있다.
>
> 영상의 모든 칸이 `1`이라면 `1`로, `0`이라면 `0`으로 압축된다. 그렇지 않다면, 영상을 4개로 분할하고 가능하면 압축한다. 가령
>
> ```
> 01
> 11
> ```
>
> 형태의 영상이 있다면 `(0111)`로 압축된다.
>
> ```
> 00000000
> 00000000
> 00001111
> 00001111
> 00011111
> 00111111
> 00111111
> 00111111
> ```
>
> 형태의 영상이 있다면 `(0(0011)(0(0111)01)1)`로 압축된다.



1. `분할`

<img src="https://user-images.githubusercontent.com/57662010/172005894-9b5f9fbd-1464-45b7-aa94-ed8e5110a4c1.jpg" alt="quad_tree" width="50%;" />

가로와 세로가 `n`인 영상 `M`의 압축 결과인 `f(y, x, n)`은 네 개의 영상 `m1`, `m2`, `m3`, `m4`의 압축 결과로 나누어볼 수 있다.

```
f(y, x, n) = {
	0, (m1 = m2 = m3 = m4 이고 그 압축 결과가 0인 경우),
	1, (m1 = m2 = m3 = m4 이고 그 압축 결과가 1인 경우),
	(m1 + m2 + m3 + m4), (그 외의 경우)
}
```

2. `정복`: 가로와 세로의 길이가 `1`인 영상 `M`의 압축 결과는 해당 칸에 적힌 숫자 그대로이다.

```python
if n == 1:
	return M[y][x]
```

3. `병합`

```python
def compress(y, x, n):
	if n == 1:
		return M[y][x]

	nn = n // 2
	m1 = compress(y, x, nn)
	m2 = compress(y, x + nn, nn)
	m3 = compress(y + nn, x, nn)
	m4 = compress(y + nn, x + nn, nn)

	if m1 == m2 == m3 == m4 and len(m1) == 1:
		return m1

	return f'({m1 + m2 + m3 + m4})'
```

[쿼드_트리.py](https://github.com/leegwae/problem-solving/blob/main/divide_and_conquer/%EC%BF%BC%EB%93%9C_%ED%8A%B8%EB%A6%AC.py)



## 참고

- [Write a program to calculate pow(x, n)](https://www.geeksforgeeks.org/write-a-c-program-to-calculate-powxn/) 참고
- 파이썬 알고리즘 인터뷰(박상길 저) - 분할 정복 참고
- [라이-분할 정복](https://blog.naver.com/kks227/220776241154) 참고
