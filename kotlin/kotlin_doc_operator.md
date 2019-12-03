# Kotlin Operators

오늘 갑자기 Kotlin에서 'in'이라던지 '..'이라던지에 대한 명칭이 궁금해지는 일이 있었는데, 그냥 네이밍을 Operator라고 지칭하는 것같다. Operator면 RX에서쓰던 것밖에 생각을 못해서 convention인가 고민했었는데 [Kotlin Document](https://kotlinlang.org/docs/reference/operator-overloading.html)에 잘 정리되어 있다.

Kotlin에서는 Operator들에 대한 오버로딩을 작업할 수 있는데, Operator는 보통 Expression의 형태로 많이 사용되고 있지만, overloading을 하기 위해서는 Member function이나 Extentionn function을 구현해야 한다. 그리고 이러한 오퍼레이터들에는 [우선순위](https://kotlinlang.org/docs/reference/grammar.html#expressions)들이 정의 되어 있으니 궁금하면 확인해보자! 아래는 간단하게 Unary, Binary Operation들을 정리해두었다.


## Unary operations

### Unary prefix operators

| Expression | Translated to |
|------------|---------------|
| `+a` | `a.unaryPlus()` |
| `-a` | `a.unaryMinus()` |
| `!a` | `a.not()` |


### Increments and decrements

| Expression | Translated to |
|------------|---------------|
| `a++` | `a.inc()` + see below |
| `a--` | `a.dec()` + see below |



## Binary operations

### Arithmetic operators 

| Expression | Translated to |
| -----------|-------------- |
| `a + b` | `a.plus(b)` |
| `a - b` | `a.minus(b)` |
| `a * b` | `a.times(b)` |
| `a / b` | `a.div(b)` |
| `a % b` | `a.rem(b)`, `a.mod(b)` (deprecated) |
| `a..b ` | `a.rangeTo(b)` |


### 'In' operator


| Expression | Translated to |
| -----------|-------------- |
| `a in b` | `b.contains(a)` |
| `a !in b` | `!b.contains(a)` |


### Indexed access operator

| Expression | Translated to |
| -------|-------------- |
| `a[i]`  | `a.get(i)` |
| `a[i, j]`  | `a.get(i, j)` |
| `a[i_1, ...,  i_n]`  | `a.get(i_1, ...,  i_n)` |
| `a[i] = b` | `a.set(i, b)` |
| `a[i, j] = b` | `a.set(i, j, b)` |
| `a[i_1, ...,  i_n] = b` | `a.set(i_1, ..., i_n, b)` |


### Invoke operator

| Expression | Translated to |
|--------|---------------|
| `a()`  | `a.invoke()` |
| `a(i)`  | `a.invoke(i)` |
| `a(i, j)`  | `a.invoke(i, j)` |
| `a(i_1, ...,  i_n)`  | `a.invoke(i_1, ...,  i_n)` |


### Augmented assignments

| Expression | Translated to |
|------------|---------------|
| `a += b` | `a.plusAssign(b)` |
| `a -= b` | `a.minusAssign(b)` |
| `a *= b` | `a.timesAssign(b)` |
| `a /= b` | `a.divAssign(b)` |
| `a %= b` | `a.remAssign(b)`, `a.modAssign(b)` (deprecated) |


### Equality and inequality operators

| Expression | Translated to |
|------------|---------------|
| `a == b` | `a?.equals(b) ?: (b === null)` |
| `a != b` | `!(a?.equals(b) ?: (b === null))` |


### Comparison operators

| Expression | Translated to |
|--------|---------------|
| `a > b`  | `a.compareTo(b) > 0` |
| `a < b`  | `a.compareTo(b) < 0` |
| `a >= b` | `a.compareTo(b) >= 0` |
| `a <= b` | `a.compareTo(b) <= 0` |
