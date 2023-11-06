# Base Conversion

10에서 36진법의 수는 10 이상의 각 자릿수를 하나의 영문자로 나타낸다. 따라서 10에서 36진법의 수에서 10 이상의 각 자릿수를 영문자로 변환하거나 그 반대를 만드는 함수를 만들어보자.

- 10 이상의 수를 영문자로 변환하기. 10 미만의 수라면 단순히 문자열로 바꾼다.

```python
def to_alphabet(decimal: int) -> str:
    if decimal < 10:
        return str(decimal)
    
    return chr(decimal - 10 + ord('A'))
```

- 영문자를 10 이상의 수로 변환하기. 10 미만의 수라면 단순히 숫자로 바꾼다.

```python
def to_decimal(alphabet: str) -> int:
    if alphabet.isdigit():
        return int(alphabet)
    
    return ord(alphabet) - ord('A') + 10
```



## 10진수 `n`을 `k`진법으로 변환하기

주어진 수 `n`가 0이 될 때까지 `k`로 나누고, 그 나머지들을 역으로 나열하면 된다.

```python
def convert(n: int, base: int):
    converted = ''
    
    while n > 0:
        converted += to_alphabet(n % base)
        n //= base
        
    return converted[::-1]
```



## `k`진수 `n`을 10진법으로 변환하기

주어진 수 `n`의 각 `i`(맨 오른쪽부터 0)번째 자릿수를 `k^i`'만큼 곱하여 더하면 된다.

```python
def convert(n: str, base: int):
    decimal = 0
    
    for i in range(len(n)):
        decimal += to_decimal(n[i]) * (base ** (len(n) - i - 1))
        
    return decimal
```