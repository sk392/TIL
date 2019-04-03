## SealedClass

Sealed Class는 enum class의 확장판이라고 말할 수 있는데, sealed class,enum class 모두 타입을 제한하고 추상화 하는데 유용한데, 다른점이 있다면 kotlin에서 enum class는 모두 같은 타입의 변수와 같은 타입의 함수를 가져야하는데, sealed class는 서로다른 타입의 변수나 함수를 가질 수 있다.


```
sealed class Option<out T> {
    object None : Option<Nothing>()
    data class Some<out T>(val value: T) : Option<T>()
}

sealed class Either<out A, out B> {
    data class Left<out A>(val value: A) : Either<A, Nothing>()
    data class Right<out B>(val value: B) : Either<Nothing, B>()
}

sealed class List<out A> {
    object Nil : List<Nothing>()
    data class Cons<out A>(val head: A, val tail: List<A>) : List<A>()
}
```

sealed class도 대수적 타입이기 때문에 타입을 추상화할 수 있어서 이를 통한 패턴 매칭이 가능!!

```
val some5 = Option.Some(5)
val none = Option.None

fun <T> sayOption(option: Option<T>): String = when (option) {
    is Option.None -> "None"
    is Option.Some -> "Some : ${option.value}"
}

println(sayOption(some5))     // Some : 5
println(sayOption(none))      // None
```


#### 정리


* Kotlin은 enum class와 sealed class는 대수적 타입이다.
* 합 타입을 패턴매칭과 표현식으로 사용하려면 모든 타입에대해 구현을 해야한다.
* 타입클래스(interface)를 사용하면 대수적타입과 비슷한 효과를 낼수 잇지만 패턴매칭와 표현식으로 사용하려면 else 구문을 추가로 구현해야 한다.
* 만약 합 타입이 추가된다면, 추가된 타입에 대한 처리도 항상 해줘야 한다. 실수로 타입만 추가하고 구현하지 않는 다면 컴파일 에러가 발생한다. 타입만 추가하고 비즈니스 로직을 구현하지 않는 실수를 컴파일 타임에 방지 할 수 있는게 장점잼