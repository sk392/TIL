# Kotlin_Generics

kotlin의 Generics을 정리 Java에서 사용하는 Generics과 동일하게 사용할 수 있지만, Kotlin은 Generics 정의한 클래스를 상속받을 때 명시적으로 지정해야 한다.

다시 말해 java에서는 제네릭 정의를 하지 않아도 기본 Object로 만들어주지만, 코틀린에서는 명시적으로 꼭 적어주도록 만들었다.

#### Java와 Kotlin의 Generics 차이


자바에서의 Generics는 

```java
//Java
interface Generic<T> {
  void setItem(T item);
}
```

이렇게 사용하는데 반해 Kotlin은

```kotlin
//Kotlin
interface Generic<in T> {
  fun setItem(item: T)
}
```

이렇게 정의하게 된다. 반드시 Generics를 지정해줘야한다. 상속받아 구현하려면 다음과 같이 하면 된다.

```kotlin
class Sample : Generic<Generic Type을 정의> {

    override fun setItem(item: Generic Type을 정의) {
        // 구현
    }
}
```


## Wildcard tpye argument

Java에는 와일드카드라 불리는 부분이 있습니다. Kotlin에서는 in/out을 명시해야 하는데 이러한 부분을 이해하기 위해서 Java의 와일드카드를 먼저 살펴보자

이러한 형태는 보통 List 내부 구현체에서 많이 볼 수 있는데 다음과 같이 addAll의 Collection에 extends로 정의한 것을 볼 수 있다. 

코틀린은 Generics을 정의할 때 in/out을 자동으로 표현할 수 있도록 워닝 메시지를 출력한다. option + enter을 함께 눌러서 in/out을 자동으로 표현할 수 있으니 참고


#### Wildcard type argument - java

먼저 Java의 와일드카드를 살펴보면, 크게 extends와 super로 구분되어 있다. 각각의 의미는 아래와 같다.

T : read/write 모두 가능
? extends T : read만 가능한 서브타입 와일드카드
? super T : write만 가능한 슈퍼 타입 와일드카드
read만 가능한 extends와 write만 가능한 super 2가지가 있다. read/write 모두를 해야 하면 와일드카드가 존재하지 않을 듯


#### Wildcard type argument - kotlin

kotlin에서는 명시적으로 super/extends 대신 in/out 키워드를 제공하는데, 각각의 정의와 매칭은 아래와 같다.

T : 별도의 Wildcard 정의가 없이 read/write 모두 가능
in T : Java의 ? super T와 같음. input의 약자이며 write 만 가능
out T : Java의 ? extends T와 같음. output의 약자이며 read 만 가능
kotlin에서도 java처럼 테스트하기 위해 generics interface을 아래와 같이 정의하였다.

```
interface Output<T> {
    fun isArgument(argument: T): Boolean
}
```

사용하는 ArrayList의 아이템을 다음과 같이 2개 정의할 수 있다.

```

class ExampleUnitTest {

    val items = ArrayList<Output<String>>()

    init {
        items.add(object : Output<String> {
            override fun isArgument(argument: String) = false
        })
        items.add(object : Output<String> {
            override fun isArgument(argument: String) = true
        })
    }
}
```


