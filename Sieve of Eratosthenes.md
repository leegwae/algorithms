# Sieve of Eratosthenes

- **에라토스테네스의 체(Sieve of Eratosthenes)**는 주어진 `n` 이하의 소수들을 모두 찾을 수 있다.



## `n` 이하의 소수를 찾는 일반적인 방법

### 어떤 수가 소수인지 판별하기

1. `p`가 소수가 아니라면, `p`는 `1`과 `p` 자신 외에 약수를 가진다. 달리 말하여 `p`가 `0`으로 나누어떨어지는 수 `i` (`1 < i < p`)가 있다.
2. 따라서 `p`가 소수인지 알려면 `2`부터 `p - 1`을 반복문으로 돌며 나누어떨어지는 확인한다.

```python
def is_prime(p):
	for i in range(2, p):
		if p % i == 0:
			return False

	return True
```



### 전체 코드

1. `2`부터 `n`까지의 수 `p`를 반복문으로 돌며 소수인지 판별한다.

```python
# n: 주어진 수
def is_prime(p):
	for i in range(2, p):
		if p % i == 0:
			return False

	return True

prime = []
for i in range(2, n + 1):
    if is_prime(i):
        prime.append(i)

print(prime)
```

시간 복잡도는 `O(n^2)`으로 상당히 비효율적이다.



## 에라토스테네스의 체의 동작

1. `2`부터 `n`까지의 수를 나열한다.
2. `2`의 배수를 지운다.
3. `3`의 배수를 지운다.
4. `4`는 `2`의 배수라 이미 지워졌으므로 건너뛴다.
5. `5`의 배수를 지운다.
6. (...생략) `n`의 배수를 지운다.

일반화하면, `2`부터 `n`까지 루프를 돌며 배수를 지운다.



```python
def eratosthenes():
    for i in range(2, N + 1):
        for j in range(i, N + 1, i):
            if isPrime[j]:
                isPrime[j] = False

N = 주어진 수
isPrime = [True] * (N + 1)
isPrime[0] = isPrime[1] = False
eratosthenes()
print(list(filter(lambda i: isPrime[i], range(2, N + 1))))
```



[예: 에라토스테네스의_체.py](https://github.com/leegwae/problem-solving/blob/main/sieve/%EC%97%90%EB%9D%BC%ED%86%A0%EC%8A%A4%ED%85%8C%EB%84%A4%EC%8A%A4%EC%9D%98%20%EC%B2%B4.py)



### 최적화된 에라토스테네스의 체

1. `2`부터 `n`의 제곱근까지 루프를 돈다: 합성수 `n`이 `p`와 `q`의 곱으로 표현될 때 하나는 `n`의 제곱근보다 크고 하나는 `n`의 제곱근보다 작을 것이기 때문이다.
2. `i`의 배수를 지울 때는 `2 * i`부터가 아니라 `i * i`부터 시작한다: `2 * i`부터 `(i - 1) * i`까지의 수는 `2`의 배수, `3`의 배수, ..., `i - 1`의 배수를 지우며 이미 지웠을 것이기 때문이다.



[optimized_sieve.py](https://github.com/leegwae/problem-solving/blob/main/sieve/optimized_sieve.py)

```python
def eratosthenes():
	for i in range(2, sqrtN):
		if isPrime[i]:
			for j in range(i * i, N + 1, i):
				isPrime[j] = False


if __name__ == '__main__':
	N = int(input())
	isPrime = [True] * (N + 1)
	sqrtN = int(N ** (1 / 2))
	eratosthenes()
	print(list(filter(lambda i: isPrime[i], range(2, N + 1))))
```



## 에라토스테네스의 체의 복잡도

### 시간 복잡도

기본적으로 `O(nlogn)`이며, 최적화한 에라토스테네스의 체는 `O(nloglogn)`으로 매우 빠르다.



### 공간 복잡도

`n`까지의 수가 소수인지 저장해야하므로 `O(n)`만큼의 공간을 필요로 한다.

