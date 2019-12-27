
# Exploring Kotlin’s hidden costs - Part.2


Kotlin의 숨겨진 코스트들이 뭐가 있는지 정리해보자  2탄의 글은 [여기](https://medium.com/@BladeCoder/exploring-kotlins-hidden-costs-part-2-324a4a50b70) 있다.


## Local functions
 
 local funciton이란 다른 function 내부에 선언된 function을 말하며 function 내부에서만 접근할 수 있다.
 
 ```
//example 1
 
fun someMath(a: Int): Int {
    fun sumSquare(b: Int) = (a + b) * (a + b)

    return sumSquare(1) + sumSquare(2)
}

 ```
 
 local function의 가장 큰 문제점은 local function은 inline으로 선언할 수 없고, local function을 포함한 function도 inline으로 선언할 수 없습니다. 이 문제는 해결할 수 없습니다.
 
 컴파일 후에 local function들은 ```Function``` Object로 변환되고, 람다와 마찬가지로 inline이 아닌 함수와 관련해서 [이전 설명](https://github.com/sk392/TIL/blob/master/kotlin/kotlin_hidden_costs.md)에서 설명한 것과 같은 제한이 생깁니다. Java로 Decompile하면 다음과 같습니다.
 
 ```
 //example 1 - Decompile

 public static final int someMath(final int a) {
   Function1 sumSquare$ = new Function1(1) {
      // $FF: synthetic method
      // $FF: bridge method
      public Object invoke(Object var1) {
         return Integer.valueOf(this.invoke(((Number)var1).intValue()));
      }

      public final int invoke(int b) {
         return (a + b) * (a + b);
      }
   };
   return sumSquare$.invoke(1) + sumSquare$.invoke(2);
}

 ```
 
 이런 방법은 실제 function의 인스턴스가 caller를 알고 있기 때문에 synthetic method 대신에 특정 method를 바로 호출할 수 있어서 Lambda에 비해서 method call이  1회 더 적습니다.
 
 즉, 외부에서 local function을 호출할 때 primitive Type의 casting이나 boxing이 발생하지 않는다는 것을 의미합니다. 바이트코드로 보면 다음과 같습니다.
 
 ```
 ALOAD 1
ICONST_1
INVOKEVIRTUAL be/myapplication/MyClassKt$someMath$1.invoke (I)I
ALOAD 1
ICONST_2
INVOKEVIRTUAL be/myapplication/MyClassKt$someMath$1.invoke (I)I
IADD
IRETURN
 ```
 
위 코드를 보면 2번 호출되는걸 볼 수 있는데, int를 인풋으로 받고 또 int를 리턴하는걸 볼 수 있습니다. 그리고 그 과정에서 unboxing하는 동작없이 즉시 수행되는 것을 알 수 있습니다.

하지만 여전히 각 메서드가 호출 될 때마다 ```Function``` Object가 생성되는 비용이 있고,   함수를 수정함으로서 capturing되지 않게 회피할 수 있습니다.

```
//example 2

fun someMath(a: Int): Int {
    fun sumSquare(a: Int, b: Int) = (a + b) * (a + b)

    return sumSquare(a, 1) + sumSquare(a, 2)
}
```

위와 같이 수정한다면 같은 Function Instance를 재사용하는 것을 알 수 있습니다. 이런 경우  local function의 패널티는 다른 function에 비해서 몇 개의 method가 있는 extra class를 사용한다는 것을 알 수 있습니다.

```
//example 2 - Decompile

public final class MyClass {
   public final int someMath(int a) {
      <undefinedtype> sumSquare$ = null.INSTANCE;
      return sumSquare$.invoke(a, 1) + sumSquare$.invoke(a, 2);
   }
}
```

> local function은 outer function(local function을 감싸는 function)의 local vaiables에 접근할 수 있다는 장점이 있고, 그 이점은 outer function을 콜할 때마다 ```Function``` object를 만든다는 비용이 들고 그로 인해서 non-capturing 방식의 local function을 선호합니다.


## Null Safety

 kotlin은 nullable 과 non-null을 명확하게 분리하는 언어이며 null이나 nullable변수를 non-null형태의 변수에 넣지 못하도록 막음으로서 runtime중에  compiler가 예기치 못한 ```NullpointerException```이 발생하는걸 방지할 수 있습니다.
 
 
### Non-null arguments runtime checks

non-null형태로 String argument를 선언해보면 

```
fun someMath(a: String) {
    println("hello $a")
}    
```

다음과 같은데 이걸 java로 decompile을 하게되면 아래와 같이 표현된다.

```
public final class MyClass {
   public final void someMath(@NotNull String a) {
      Intrinsics.checkParameterIsNotNull(a, "a");
      String var2 = "hello " + a;
      System.out.println(var2);
   }
}
```

kotlin의 non-null type은  @NotNull annotation을 붙여서 Java에서 해당 function을 호출할 때에 warning을 노출해서 null value가 넘어오지 않도록 방지합니다.

하지만 annotation로 null safety를 보장하기는 충분하지 않습니다. 그래서 compiler는 맨 앞에 null check하는 static method를 추가시켜 IllegalArgumentException을 발생시킵니다. null check를 통해서 빠르게 고칠 수 있도록 추후에 발생 가능한 NullPointerException보다 빠르게 에러를 뱉어줍니다.

실제로 모든 public function들은 모든 non-null argument에 ```Intrinsics.checkParameterIsNotNull()```가 추가됩니다. private function들에는 별도로 null check가 진행되지 않는데, compiler가 kotlin class 안에 있는 코드는 null safety를 보장하기 때문입니다.

static function을 호출하는 일은 performance에 큰 영향을 미치진 않으며 디버깅이나 테스트에 유용하기 때문에 필요하다면 사용하는 것이 더 좋습니다. 하지만 이런 것들은 relese build할 때 필요하지 않다고 생각할 수 도 있고(검증이 완료되었기 때문에) 그렇다면 runtime null checks를 끌 수 있도록 compiler option을 ProGuard 옵션에 ```-Xno-param-assertions```를 넣어주면 됩니다.

```
-assumenosideeffects class kotlin.jvm.internal.Intrinsics {
    static void checkParameterIsNotNull(java.lang.Object, java.lang.String);
}
```

> 위에 ProGuard는 optimization이 켜져있을 때만 동작합니다. android ProGuard는 기본적으로 꺼져있으니 반드시 optimization을 켜고 옵션을 넣기 바랍니다.

### Nullable primitive types

 nullable type은 기본적으로 전부 reference type이지만, primitive type을 nullable하게 선언하면 어떻게 될까요? 아마 짐작하셨겠다 싶이 int나 float같은 primitive type을 사용하지 않고 Boxed reference Type인 ```Integer```나 ```Float```을 사용하게 되고 이렇게 되면 boxing, unboxing에 추가적인 비용이 들어갑니다.
 
 자바와는 달리 autoboxing과 null에 대한 걱정 없이 **```int```** 와 거의 똑같이 ```Interger``` 변수를 사용할 수 있습니다. 
 
```
fun add(a: Int, b: Int): Int {
    return a + b
}
fun add(a: Int?, b: Int?): Int {
    return (a ?: 0) + (b ?: 0)
}
```

>nullable을 보다 non-null primitive type을 사용할 때 더 코드는 명확해지고 더 나은 퍼포먼스를 보여줄 수 있습니다.


### About arrays

kotlin에는 3가지 type의 arrays가 있는데요,

* ```IntArray```, ```FloatAray``` 등 : primitive type의 Array이고 compile될 때 int[], float[] 등으로 컴파일 된다.
* ```Array<T>``` : non-null object references로 array가 구성되며 primitive type의 경우 boxing이 진행된다.
* ```Array<T?>``` : nullable object refrences로 array가 구성되고 primitive type의 경우 위와 같이 boxing이 진행된다.


> 만약 non-null primitive의 리스트가 필요하다면 IntArray가 Array<Int>보다 더 좋은 퍼포먼스를 낼 수 있다. 



## Varargs

 kotlin에는 Java처럼 [가변 개수의 argmuments](https://kotlinlang.org/docs/reference/functions.html#variable-number-of-arguments-varargs)를 사용할 수 있습니다.
 
```
fun printDouble(vararg values: Int) {
    values.forEach { println(it * 2) }
}
```

 자바와 마찬가지로 compile될 때 주워진 타입을 array형태로 받고 있고 이를 사용하기 위한 방법을 총 3가지 입니다. 예를 들어 위에 ```printDouble```을 사용해본다면
 

1. **Passing multiple arguments**

```
printDouble(1, 2, 3)
```

kotlin compiler가 compile한 코드를 Java로 Decompile하면 다음과 같이 됩니다.

```
printDouble(new int[]{1, 2, 3});
```

이는 새로운 array를 만드는 오버헤드가 생기지만 java와 다른 차이점은 없습니다.

2. **Passing a single array**

이 방법에서는 조금 다른데, Java에서는 vararg argument에 array reference를 직접적으로 넣을 수 있었지만, kotlin에서는 spread operator가 필요합니다.

```
val values = intArrayOf(1, 2, 3)
printDouble(*values)
```

java에서는 array reference를 별도의 array allocation을 사용하지 않고 있는 그대로 넣을 수 있습니다. 하지만 kotlin에서는 spread operator를 사용해서 컴파일하면 다른 결과가 나오게 됩니다. 위 코드를 Java로 Decompile하게 되면 다음과 같이 나옵니다.

```
int[] values = new int[]{1, 2, 3};
printDouble(Arrays.copyOf(values, values.length));
```

원래 있던 array를 copy해서 넣는 것을 볼 수 있습니다. 물론 이 경우에 코드가 더 안전하다고 볼 수 있습니다. function 내부의 변경에 따라 arguments를 넣어주는 곳의 값이 바뀌지 않기 때문인데, 이것은 추가적인 메모리를 사용을 발생시킵니다.

> 위 처럼 java에서 짜여진 코드를 kotlin으로 호출한다면 같은 효과가 있습니다.


3. **Passing a mix of arrays and arguments**

spread operator의 가장 큰 장점은 바로 array와 arguments를 섞어서 쓸 수 있다는 것입니다.

```
val values = intArrayOf(1, 2, 3)
printDouble(0, *values, 42)
```

compile된 것을 Java로 Decompile하면 다음과 같이 노출됩니다.

```
int[] values = new int[]{1, 2, 3};
IntSpreadBuilder var10000 = new IntSpreadBuilder(3);
var10000.add(0);
var10000.addSpread(values);
var10000.add(42);
printDouble(var10000.toArray());
```

이 로직은 array를 새로 하나 만드는 것 외에도 builder object를 써서 최종적으로 사이즈를 계산하고 새로운 array로 사용하지만 추가적인 비용이 발생됩니다.

> kotlin에서 ```vararg``` 사용하는 것은 spread를 사용하던 사용하지 않던 항상 임시 array를 만들기 때문에 추가적인 비용을 발생시킵니다. 가장 크리티컬한 경우는 이 방식을 반복적으로 호출할 때 입니다. 이런 경우라면 ```vararg```를 사용하는 것 외에 별도의 method들을 만드는 것이 좋습니다.