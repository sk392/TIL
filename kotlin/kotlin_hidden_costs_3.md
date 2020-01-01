
# Exploring Kotlin’s hidden costs - Part.3


Kotlin의 숨겨진 코스트들이 뭐가 있는지 정리해보자  3탄의 글은 [여기](https://medium.com/@BladeCoder/exploring-kotlins-hidden-costs-part-3-3bf6e0dbf0a4) 있습니다.


## Delegated properties

[Delegated properties](https://kotlinlang.org/docs/reference/delegated-properties.html)는 getter와 optional setter를 external object에서 주입 받는 property이며 재사용 가능한 custom property를 가지고 있습니다.

```kotlin
class Example {
    var p: String by Delegate()
}
```

delegate object는 읽기 쓰기를 위해서 ```getValue()```와 ```setValue()```라는 function을 구현해야하고 이런 function은 instance와 property metadata를 추가로 받아야 합니다.

class가 delegated property를 선언할 때 코드를 java로 Decomplie한 코드를 보면

```java
public final class Example {
   @NotNull
   private final Delegate p$delegate = new Delegate();
   // $FF: synthetic field
   static final KProperty[] $$delegatedProperties = new KProperty[]{(KProperty)Reflection.mutableProperty1(new MutablePropertyReference1Impl(Reflection.getOrCreateKotlinClass(Example.class), "p", "getP()Ljava/lang/String;"))};

   @NotNull
   public final String getP() {
      return this.p$delegate.getValue(this, $$delegatedProperties[0]);
   }

   public final void setP(@NotNull String var1) {
      Intrinsics.checkParameterIsNotNull(var1, "<set-?>");
      this.p$delegate.setValue(this, $$delegatedProperties[0], var1);
   }
}
```

일부 static property metadata 가 클래스가 추가된 것을 볼 수 있습니다. delegate는 클래스 생성자에서 초기화 되었고, 읽거나 쓸 때마다 해당 delegate가 호출되는 것을 알수 있습니다.

### Delegate instnaces

위에 예제에서 property를 구현할 때 delegate instance도 만들어지며 delegate의 구현이 stateful할 땐 반드시 필요합니다. 예를 들자면 계산된 로컬 밸류를 캐쉬할 때

```kotlin
class StringDelegate {
    private var cache: String? = null

    operator fun getValue(thisRef: Any?, property: KProperty<*>): String {
        var result = cache
        if (result == null) {
            result = someOperation()
            cache = result
        }
        return result
    }
}
```

또한 생성자를 통해 전달된 추가 parmater가 필요한 경우 새 delegate instance를 만들어야 합니다.  

```kotlin
class Example {
    private val nameView by BindViewDelegate<TextView>(R.id.name)
}
```

하지만 어떤 경우엔 property에 단 하나의 delegate instance가 필요한 경우가 있는데, delegate instance가stateless하고 변수라면 이미 object instance와 property name을 포함하고 있습니다. 그 경우 class대신에  Object로 delegate를 singleton으로 만들 수 있습니다.

예를 들면, singleton delegate가 태그 이름이 Android Activity내에서 property 이름과 일치하는 Fragment를 찾을 떄

```kotlin 
object FragmentDelegate {
    operator fun getValue(thisRef: Activity, property: KProperty<*>): Fragment? {
        return thisRef.fragmentManager.findFragmentByTag(property.name)
    }
}
```

마찬가지로 모든 기존의 object도 delegate와 같이 될 수 있고,```getValue()```와 ```setValue()```는 extention function으로 선언될 수 있습니다. kotlin은 이미 ```Map```과 ```MutableMap```을 delegate로 사용할 수 있도록 extention function을 제공하고 있습니다.

동일한 local delegate instance를 재사용해서 같은 클래스에서 여러 property를 구현하도록 선택한 경우, 클래스 생성자에서 delegate instance를 초기화 해주어야합니다.

>클래스에 선언된 각 delegate property에는 delegate object를 만드는 오버헤드가 포함되어 있으며, 클래스에도 일부 metadata가 추가 됩니다.
>
>가능하다면 다른 property대해 delegate를 재사용하는 것이 좋습니다.
>또한 많은 수의 delegate property를 선언해야하는 경우 최선인지 고려해봐야 합니다.


### Generic delegates

Delegate function은 일반적인 방식으로 선언될 수 있으므로, 같은 delegate class에서 다양한 타입의 property와 함께 사용할 수 있습니다.

```kotlin
private var maxDelay : Long by SharedPreferencesDelegate <Long> ()
```

그러나 위와 같이 primitive 타입과 함께 generic delegate와 함께 사용하는 경우, 선언된 primitive 타입이 nullable이 아닌 경우에도 읽거나 쓸 때마다 boxing 및 unboxing이 발생합니다.

> non-null primitive 타입의 delegate property에 경우 property에 엑세스할 때 마다 오버헤드가 발생하지 않도록 generic delegate를 사용하지 않고 각 유형에 대해 특수화된 delegate class는 것이 좋습니다.


### Standard delgates : lazy()

kotlin 몇가지 표준 delegate 형식을 지원해주는데, 그것은 다음과 같습니다. ```Delegates.notNull()```,```Delegates.observable()```,```lazy()```

```lazy()```는 value 타입일 때만 사용 가능하며, 처음 읽을 때 속성 값을 제공 된 람다를 통해 값이 초기화 되도록 대리자를 반환하는 함수입니다.

이 방식은 실제로 필요할 때까지 리소스가 많이 들어가는 초기화를 연기해서 코드를 읽기 쉽게하면서도 성능을 향상 시키는 깔끔한 방법입니다.

또한 ```lazy()```는 inline function이 아니며 별도로 전달된 lambda는 ```Function``` class로 컴파일 되며 delegate object안에는 inline되지 않습니다.

종종 간과 하는 것은 ```lazy()```에 3가지 다른 유형의 delegate를 반환하는 optional ```mode```가 존재합니다.


```kotlin
public fun <T> lazy(initializer: () -> T): Lazy<T> = SynchronizedLazyImpl(initializer)
public fun <T> lazy(mode: LazyThreadSafetyMode, initializer: () -> T): Lazy<T> =
        when (mode) {
            LazyThreadSafetyMode.SYNCHRONIZED -> SynchronizedLazyImpl(initializer)
            LazyThreadSafetyMode.PUBLICATION -> SafePublicationLazyImpl(initializer)
            LazyThreadSafetyMode.NONE -> UnsafeLazyImpl(initializer)
        }
```

기본 모드는 ```LazyThreadSafetyMode.SYNCHRONIZED```인데, 상대적으로 많은 리소스를 사용하는 double-checked lock을 사용합니다. 이는 mutli thread 환경에서 property를 읽을 때 안전하게 실행되도록 보장하는데 필요합니다.

만약 main thread와 같이 단일 스레드에서만 property에 접근한다면 명시적으로 ```LazyThreadSafetyMode.NONE ```를 사용해서 lock을 완전히 피할 수 있습니다.

```kotlin
val dateFormat: DateFormat by lazy(LazyThreadSafetyMode.NONE) {
    SimpleDateFormat("dd-MM-yyyy", Locale.getDefault())
}
``` 

> ```lazy()```  delegate를 사용해서 많은 리소스를 잡아먹는 초기화를 연기하고 ```LazyThreadSafetyMode ```를 지정해서 불필요한 lock을 피하는 것이 좋습니다.


## Ranges

 [Ranges](https://kotlinlang.org/docs/reference/ranges.html)는 유한한 값 세트를 나타내는데 사용하는 kotlin의 특수한 표현식이며 여기서 사용하는 값은 모든 ```Comparable``` 타입이 될 수 있습니다. 이 표현식은 ```ClosedRange``` 인터페이스를 구현하는 객체를 생성한느 함수로 구성되어 있습니다. 범위를 만드는데 사용되는 주요 기능은 ```..``` operator를 사용합니다.
 
### Inclusion tests
 
 범위 표현식의 주요 목적은 in 및 !in연산자를 사용합니다.
 
 ```kotlin
 if (i in 1..10) { 
    println (i) 
}
 ```
 
 구현은 non-null primitive type에 최적화 되어 있으며(Int, Long, Byte, Short, Float, Double or Char values) 위에 예시는 다음과 같이 컴파일된 것을 볼 수 있다.
 
 ```java
 if (1 <= i && i <= 10) { 
   System.out.println (i); 
}

 ```
 
 위 작업은 overhead와 별도의 객체할당이 없으며 다른 non-null primitive Comparable type도 동일하게 동작하며 ```when``` 과 함께 사용하면 더 명료한 코드를 작성할 수 있습니다.
 
 ```kotlin
 val message = when (statusCode) {
    in 200..299 -> "OK"
    in 300..399 -> "Find it somewhere else"
    else -> "Oops"
}
 ```

하지만 변수 선언과 사용 사이에 하나 이상의 단계가 추가된다면 작지만 리소스를 더 사용하게 됩니다.

```kotlin
private val myRange get() = 1..10

fun rangeTest(i: Int) {
    if (i in myRange) {
        println(i)
    }
}
```

위에 경우엔 ```IntRange```를 컴파일 한 후에 별도의 객체를 추가 생성하게 됩니다.

```java
private final IntRange getMyRange() {
   return new IntRange(1, 10);
}

public final void rangeTest(int i) {
   if(this.getMyRange().contains(i)) {
      System.out.println(i);
   }
}
```

property를 ```inline```으로 생성하더라도 추가 객체 생성을 막을 수 없습니다.

>range object를 추가적으로 생성하지 않게끔 간접적으로 사용되지 않는 것들은 직접 범위에 선언하는게 좋습니다. 또는 추가 객체 생성을 줄이기 위해서 const형태로 재사용을 하는 것이 좋습니다.


### iterations: for loops

Integral type Ranges(primitive type Range중에서 ```Float```과 ```Double```을 제외한 타입들)을 사용해서 반복 될 수 있습니다. 이를 통해서 Java의 클래식한 for 루프를 더 짧게 만들 수 있습니다.

```kotlin
for (i in 1..10) {
    println(i)
}
```

위에 코드는 아래와 같이 overhead가 없는 최적화된 코드로 컴파일 됩니다.

```java
int i = 1;
for(byte var2 = 11; i < var2; ++i) {
   System.out.println(i);
}
```

역순으로 하고 싶다면 ```..``` 대신에 ```downTo()```를 사용하면 됩니다.

```kotlin
for (i in 10 downTo 1) {
    println(i)
}
```

이 방식 또한 overhead가 전혀 없습니다.

```java
int i = 10;
byte var1 = 1;
while(true) {
   System.out.println(i);
   if(i == var1) {
      return;
   }
   --i;
}
```

상한 값을 제외하고 반복하고 싶을 땐 ```until()```을 사용할 수 있습니다.

```kotlin
for (i in 0 until size) {
    println(i)
}
```

이 방법도 koltin 1.1.4 이후에 개선되어 위 처럼 아무런 overhead도 존재하지 않습니다.

하지만 다른 변형된 iteration들은 최적화되어 있지 않습니다.

예를 들면 ```downTo()```와 동일한 결과를 낳는 방법중에 하나인 ```reversed()```를 사용하면 최적화되지 않아 

```kotlin
for (i in (1..10).reversed()) {
    println(i)
}
```

이 코드를 Java로 Decompile한 경우 다음과 같다.

```java
IntProgression var10000 = RangesKt.reversed((IntProgression)(new IntRange(1, 10)));
int i = var10000.getFirst();
int var3 = var10000.getLast();
int var4 = var10000.getStep();
if(var4 > 0) {
   if(i > var3) {
      return;
   }
} else if(i < var3) {
   return;
}

while(true) {
   System.out.println(i);
   if(i == var3) {
      return;
   }

   i += var4;
}
```

범위를 나타내기 위해서 ```IntRange``` 객체를 임시로 만들고, 그 객체를 반대로 하기 위해서 ```IntProgression``` 객체를 만듭니다.

이렇게 비슷한 기능을 만들기 위해서 둘 이상의 함수를 조합하게 되면 최소 2개의 가벼운 객체를 만드는 overhead 코드들이 추가됩니다.

마찬가지로 ```step()```을 사용하는 것도 overhead 코드들이 추가됩니다.

```kotlin
for (i in 1..10 step 2) {
    println(i)
}
```

참고로 위와 같은 코드가 ```IntProgression```의 속성을 읽을 때, 범위 내에 정확한 마지막 값을 결정하기 위해서 작은 계산을 수행하게 됩니다.

> ```for```루프를 포함하는 다양한 표현 중에서 ```..```, ```downTo().```, ```.until()``` 등 단일 함수를 호출하는게 임시 Progression Object를 만들지 않고 추가적인 overhead를 피할 수 있습니다.


## Iterations: forEach()

```for```대신에 자주 사용할 수 있는 ```forEach()``` 에서도 비슷한 결괄르 얻기 위해서 인라인 확장 기능을 사용할 수 있습니다.

```kotlin
(1..10).forEach {
    println(it)
}
```

하지만 ```forEach()```에 사용된 함수의 시그니쳐를 자세히 살펴보면 범위에 최적화되어 있지 않고 ```Iterable```에 대해서만 최적화가 되어 있기 때문에 별도의 iterator를 만들어야 합니다. Java로 Decompile된 코드는 다음과 같습니다.

```java
Iterable $receiver$iv = (Iterable)(new IntRange(1, 10));
Iterator var1 = $receiver$iv.iterator();

while(var1.hasNext()) {
   int element$iv = ((IntIterator)var1).nextInt();
   System.out.println(element$iv);
}
```

위에 코드를 보면 이전에 ```for```에서 설명했던 코드보다 훨씬 효율적이지 않습니다. ```IntRange``` 객체를 만드는 것외에도 ```IntIterator``` 객체를 만드는 것을 알 수 있습니다.

> 특정 범위를 반복하고 싶을 때는 간단한 ```for```을 사용하는 것이 좋습니다. ```forEach()```는 iterator object를 생성하기 때문에 성능상 ```for```가 좋습니다.


### Iteration: collection indices

Kotlin 표준라이브러리는 [```indices```](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/indices.html) 확장 속성을 array와 collection에 제공해주고 있습니다.

```kotlin
val list = listOf("A", "B", "C")
for (i in list.indices) {
    println(list[i])
}
```

놀랍게도 위의 ```indices```를 compile하면 최적화된 코드로 변환 됩니다 

```java
List list = CollectionsKt.listOf(new String[]{"A", "B", "C"});
int i = 0;
for(int var2 = ((Collection)list).size(); i < var2; ++i) {
   Object var3 = list.get(i);
   System.out.println(var3);
}
```

여기서 ```IntRange```객체를 전혀 생성하지 않은 것을 볼 수 있으며 가능한 한 효율적으로 작성되었습니다.

이 것을 ```Collection```을 구현하는 배열과 클래스에서 잘 작동하는 것을 볼 수 있고, 따라서 같은 성능을 기대하면서 커스텀 class에서 ```indices``` extention 속성을 사용하고 싶을 수 있습니다.


```kotlin
inline val SparseArray<*>.indices: IntRange
    get() = 0 until size()

fun printValues(map: SparseArray<String>) {
    for (i in map.indices) {
        println(map.valueAt(i))
    }
}
```

하지만 compile된 후에 코드를 보면 range object에 대해 알아서 피할 수 없기 때문에 효율적이지 않다는 것을 알 수 있습니다.

```java
public static final void printValues(@NotNull SparseArray map) {
   Intrinsics.checkParameterIsNotNull(map, "map");
   IntRange var10000 = RangesKt.until(0, map.size());
   int i = var10000.getFirst();
   int var2 = var10000.getLast();
   if(i <= var2) {
      while(true) {
         Object $receiver$iv = map.valueAt(i);
         System.out.println($receiver$iv);
         if(i == var2) {
            break;
         }
         ++i;
      }
   }
}
```

대신에 ```until()```을 사용해서 간단히 회피할 수 있습니다.

```kotlin
fun printValues(map: SparseArray<String>) {
    for (i in 0 until map.size()) {
        println(map.valueAt(i))
    }
}
```

> ```Collection``` 인터페이스를 상속받지 않은 custom collection을 사용하는 경우 ```for``` 안에서 함수 또는 속성에 의존하기 보다는 직접적으로 인덱스를 호출해서 사용해서 range object를 생성하는걸 회피하는게 좋습니다.