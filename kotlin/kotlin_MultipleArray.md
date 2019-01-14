## kotlin_MultipleArray


kotlin에서는 자바처럼 int[][] array = new int[5][5]형식으로 간단하게 만들 수 없고 상대적으로 어렵다(뭔가 보편적으론 코틀린이 더 간단한데)

우선 kotlin에서는 기본적으로 초기 값을 가지는 원하는 크기의 array는 다음과 같이 사용할 수 있다.(사실 이런 식으로 사용하는 것은 처음이다)

```
//Array(size: Int, init: (Int) -> T)
val array =Array(5, i -> i+2)
//[2,3,4,5,6]
```

그래서 Multiple Array는 다음과 같이 만들 수 있다

```
//2D
val array = Array(5, {Array(5,{i -> i * 2})})
/*
[{0,2,4,6,8},
{0,2,4,6,8},
{0,2,4,6,8},
{0,2,4,6,8},
{0,2,4,6,8}]
*/
```

를 좀 더 코틀린스럽게 바꾸면

```
val array = Array(5){Array(5){i -> i * 2}}

```
로 코드가 조금 더 깔끔해졌다.


#### Array (in kotlin)

코틀린에서 정의된 Array는 다음과 같다 

```

/**
 * Represents an array (specifically, a Java array when targeting the JVM platform).
 * Array instances can be created using the [arrayOf], [arrayOfNulls] and [emptyArray]
 * standard library functions.
 * See [Kotlin language documentation](http://kotlinlang.org/docs/reference/basic-types.html#arrays)
 * for more information on arrays.
 */
public class Array<T> {
    /**
     * Creates a new array with the specified [size], where each element is calculated by calling the specified
     * [init] function. The [init] function returns an array element given its index.
     */
    public inline constructor(size: Int, init: (Int) -> T)

    /**
     * Returns the array element at the specified [index]. This method can be called using the
     * index operator:
     * ```
     * value = arr[index]
     * ```
     */
    public operator fun get(index: Int): T

    /**
     * Sets the array element at the specified [index] to the specified [value]. This method can
     * be called using the index operator:
     * ```
     * arr[index] = value
     * ```
     */
    public operator fun set(index: Int, value: T): Unit

    /**
     * Returns the number of elements in the array.
     */
    public val size: Int

    /**
     * Creates an iterator for iterating over the elements of the array.
     */
    public operator fun iterator(): Iterator<T>
}

```