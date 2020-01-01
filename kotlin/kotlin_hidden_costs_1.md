
# Exploring Kotlin’s hidden costs - Part.1


Kotlin의 숨겨진 코스트들이 뭐가 있는지 정리해보자 원작자의 글은 [여기](https://medium.com/@BladeCoder/exploring-kotlins-hidden-costs-part-1-fbb9935d9b62) 있다.



#### 들어가기 앞서

 kotlin은 자바에 비해 syntactic sugar가 많이 들어가 있습니다. (문법에 생략이나 기능들이 많이 들어가있다는 의미) 또 'Black Magic'이 더 많이 존재하기 때문에 저가형 폰이나 옛날 기기에서는 무시할 수 없는 비용이 된다.  또  이 글은 Kotlin 1.1을 기준으로 작성되었고 JVM/ Android에 초점이 맞춰져 있습니다. 
 
 * **Kotlin bytecode inspector** - Android Studio에서 kotlin plugin이 설치된 상태에서 show kotlin bytecode를 선택하면 kotlin -> bytecode로 전환하고 decompile을 누르면 java코드로 변환시켜서 어떤 코드로 동작하는지 알 수 있음
 * '**Magic**' - 프로그래밍에서 Magic이란 난해한 이론 지식을 기반으로 복잡한 로직으로 구성된 코드로 이해하기 어렵지만 잘 돌아가는 코드를 의미한다.
 * '**Black Magic**' - Magic중에서 설명이 모호하거나 문서화하지 않아서 이해할 수 없거나 숨겨진 코드를 의미한다.

 
 
 
## Higher-order functions and Lambda
 
 * Higher-order functions(고차함수) - 변수에 함수를 할당하고 다른 함수에 인수로 전달하는 함수를 Higher-order functions라고 한다.
 * Lambda expressions(람다식) - :: 코드 블록앞에 이름이 붙거나 익명 함수로 선언되거나 하는 함수를 람다식 구분

 Kotlin은 Java 6/7에서 람다를 지원할 수 있는 아주 좋은 방법입니다. 예를 들어 데이터 베이스 트랜잭션 내에서 임의의 조작을 수행하고 결과를 받는 함수를 예를 들자면 다음과 같은 코드를 작성 할 수 있습니다
 

```kotlin
 fun transaction(db: Database, body: (Database) -> Int): Int {
    db.beginTransaction()
    try {
        val result = body(db)
        db.setTransactionSuccessful()
        return result
    } finally {
        db.endTransaction()
    }
}
```
 
사용하는 쪽에서는 Groovy와 유사한 구문을 사용하여 람다식을 마지막 인수로 전달하면서 호출 하면 됩니다

```kotlin
val deletedRows = transaction(db) {
    it.delete("Customers", null, null)
}
```

java6 JVM에서와 같이 람다식을 지원하지 않는 곳에서는 어떤식으로 바이트 코드가 생성될까요? 예상한대로 람다 및 익명함수는 function객체로 컴파일이되는걸 확인할 수 있습니다.

### Function Object

 컴파일 후에 람다식의 표현은 아래와 같습니다
 
 ```java
 class MyClass$myMethod$1 implements Function1 {
   // $FF: synthetic method
   // $FF: bridge method
   public Object invoke(Object var1) {
      return Integer.valueOf(this.invoke((Database)var1));
   }

   public final int invoke(@NotNull Database it) {
      Intrinsics.checkParameterIsNotNull(it, "it");
      return it.delete("Customers", null, null);
   }
}
 ```
 Android Dex파일에서 함수로 컴파일 된 각 람다식은 3개에서 4개의 메서드를 추가합니다.
 
 그래도 좋은 소식이라면 Function객체가 새인스턴스가 필요할 때만 생성 된다는건데
 
 * 캡쳐 방식은 콜 되었을 때 Function객체가 가비지컬렉터에 인수로 전달되었다면(릴리즈 될 때마다) 새로 생성된다.
 * 논 캡쳐 방식은 Function인스턴스를 Sigleton으로 생성되고 다음에 콜 될 때 다시 사용 됩니다.

다행이도 이 경우는 캡처되지 않는 람다를 사용하므로 내부 클래스가 아닌 싱글톤으로 컴파일 됩니다.
 
 
```this.transaction(db, (Function1)MyClass$myMethod$1.INSTANCE);```
 
 
<span style="background-color: rgba(0, 255, 0, 0.3);"> **인라인 형태가 아닌 higher-order 함수를 반복적으로 호출하면 캡처 방식으로 사용되므로 가비지 컬렉터에 큰 부담이 생길 수 있습니다.** </span>



### Boxing overhead

 Function Object를 컴파일할 때 Java8에서는 Boxing과 Unboxing의 오버헤드를 최대한 피하기 위해 [43개의 서로 다른 특수 함수 인터페이스](https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html)가 있는 것에 비해 Kotlin은 하나의 인터페이스만 구현되어 있습니다.

```java
/** A function that takes 1 argument. */
public interface Function1<in P1, out R> : Function<R> {
    /** Invokes the function with the specified argument. */
    public operator fun invoke(p1: P1): R
}
```

그 말은 Function Object가 Higher-order function에 전달될 때 함수의 운풋이나 리턴값이 primitive type인 경우 ststematic boxing, unboxing이 이루어질 수 있다는 말입니다.


그렇게 컴파일도니 람다의 결과가 Integer Object로 boxing한 것을 볼 수 있습니다. 그리고 이 코드를 호출한 코드에서는 unboxing해서 결과를 받을 것입니다.


### inline function to the rescue

 다행히도 Kotlin에서는 higher-odrer function을 통해 람다식을 사용할 때 이러한 overhead가 발생하지 않도록 훌륭한 방법을 제시하고 있는데 그게 바로 ```inline``` 입니다. 
 이렇게하면 호출을 완전히 피하면서도 인수로 전달된 람다식 본문이 인라인 되기 때문에 다음과 같은 이점이 있습니다.
 
 * Function Lambda가 선언될 때 객체가 인스턴스화 되지 않습니다.
 * primitive Type을 대상으로 할 때 Lambda 입력 및 리턴 값에는 boxing ,unboxing이 적용되지 않습니다.
 * 총 메서드 수에 메서드가 추가되지 않습니다.
 * 실제 함수 호출은 수행되지 않습니다. 이 함수를 반복적으로 사용할 때 CPU-heavy code들에 보다 높은 성능 향상을 시킬 수 있습니다.

이걸로 위의 ```transaction()``` function을 inline으로 변경하면 Java의 표현이 효과적으로 변환됩니다.

```java
db.beginTransaction();
try {
   int result$iv = db.delete("Customers", null, null);
   db.setTransactionSuccessful();
} finally {
   db.endTransaction();
}
```
 
 하지만 이 기능에도 **몇 가지 조심해야하는 사항**이 있습니다.
 
 * inline함수는 자기자신이나 다른 inline함수를 부를 수 없습니다.
 * public으로 선언된 inline함수는 해당 클래스의 public함수나 public필드에만 접근할 수 있습니다.
 * inline으로 구현하면 코드가 직접 들어가기 때문에 절대적인 코드 크기는 커집니다. 그리고 또한 이 inline에 속한 함수가 코드가 긴 함수라면 코드 길이는 더더욱 늘어날 수 있습니다.

 
## Companion objects

kotlin에는 static field나 method가 없어서 대신에 인스턴스와 관련이 없는 field나 method를 companion ojbect안에 선언할 수 있습니다.

* synthetic method - kotlin을 JVM에서 컴파일 하기 위해서 컴파일러가 만드는 method

### Accessing private class field from its companion object

예를 들어 다음과 같은 코드가 있을 때

```kotlin
class MyClass private constructor() {
	private var hello = 0
	
	companion object {
		fun newInstance() = MyClass()
	}
}
```

위에 코드는 컴파일 될 때 singletone class로 구현되는데, companion object에서 각각의 field나 mothod에 접근할 때마다 getter 와 setter method가 생성된다. 요렇게만 보면 무슨 말인지 이해할 수 없다 ~~(아니면 천재)~~  아래와 같이 코드에 따라 변화되는 kotlin코드와 Decompile된 코드를 정리해봤다.


```kotlin
//example 1

class MyClass private constructor() {
	private var hello = 0
	
	companion object {
		fun newInstance() = MyClass()
	}
}

```

```java
//example decompile java


public final class MyClass {
   private int hello;
   public static final MyClass.Companion Companion = new MyClass.Companion((DefaultConstructorMarker)null);
	
   private MyClass() {
   }
	
   // $FF: synthetic method
   public MyClass(DefaultConstructorMarker $constructor_marker) {
      this();
   }

   public static final class Companion {
      @NotNull
      public final MyClass newInstance() {
         return new MyClass((DefaultConstructorMarker)null);
      }
	
      private Companion() {
      }
	
      // $FF: synthetic method
      public Companion(DefaultConstructorMarker $constructor_marker) {
         this();
      }
   }
}


```

```kotlin
//example 2
	
class MyClass {
	
    private var hello = 0
	
    companion object {
        fun newInstance() = MyClass()
    }
}
```


```java
//example decompile java


public final class MyClass {
   private int hello;
   public static final MyClass.Companion Companion = new MyClass.Companion((DefaultConstructorMarker)null);
	

   public static final class Companion {
      @NotNull
      public final MyClass newInstance() {
         return new MyClass();
      }
	
      private Companion() {
      }
	
      // $FF: synthetic method
      public Companion(DefaultConstructorMarker $constructor_marker) {
         this();
      }
   }
}


```

위처럼 ```MyClass private constructor()``` 를제거하고 public으로 전환하면 불필요한 synthetic getter / setter가 생성되지 않는 것을 볼 수 있다.  


### Accessing constants decleared in a companion object

Koltin에서는 일반적으로 클래스에서 사용하는 static 상수를 companion object안에 선언하는데 보통 다음과 같이 사용했다고 가정했을 때

```
class MyClass {

    companion object {
        private val TAG = "TAG"
    }

    fun helloWorld() {
        println(TAG)
    }
}
```

위에서 언급했던 것과 마찬가지로 companion object에 private으로 선언된 상수에 companion object안에 추가적인 synthetic getter가 생성되는 것을 확인할 수 있습니다.

```
GETSTATIC be/myapplication/MyClass.Companion : Lbe/myapplication/MyClass$Companion;
INVOKESTATIC be/myapplication/MyClass$Companion.access$getTAG$p (Lbe/myapplication/MyClass$Companion;)Ljava/lang/String;
ASTORE 1
```

하지만 synthetic mothed는 실제로 값을 반환하지 않기 때문에 kotlin이 만든 getter method를 부르게 됩니다.

```
ALOAD 0
INVOKESPECIAL be/myapplication/MyClass$Companion.getTAG ()Ljava/lang/String;
ARETURN
```

상수를 public으로 전환하게 되면 getter method를 직접 호출 할 수 있으므로 synthetic method가 필요 없지만 kotlin은 아직 getter method를 호출해야합니다.

kotlin에선 상수 값을 저장하기 위해서 companion object가 아닌 myclass에 ```private static final```로 상수를 생성하게 됩니다. 하지만 이 상수는 private으로 선언 되어있기 때문에, companion object에서 접근하기 위해서는 synthetic method가 필요하게 됩니다.

```
INVOKESTATIC be/myapplication/MyClass.access$getTAG$cp ()Ljava/lang/String;
ARETURN
```

그리고 syntehtic method는 값을 읽을 수 있게 됩니다.

```
GETSTATIC be/myapplication/MyClass.TAG : Ljava/lang/String;
ARETURN
```

즉, kotlin 클래스에서 companion object의 private 상수를 읽으려면 해당 상수에 접근하지 못하고 다음과 같은 절차를 밟게 됩니다.

* companion object에서 static method를 호출
* companion object에서 instance method를 호출
* class에서 static method를 호출
* static field를 읽고 반환

이 코드를 자바로 전환해보면 다음과 같습니다.

```
public final class MyClass {
    private static final String TAG = "TAG";
    public static final Companion companion = new Companion();

    // synthetic
    public static final String access$getTAG$cp() {
        return TAG;
    }

    public static final class Companion {
        private final String getTAG() {
            return MyClass.access$getTAG$cp();
        }

        // synthetic
        public static final String access$getTAG$p(Companion c) {
            return c.getTAG();
        }
    }

    public final void helloWorld() {
        System.out.println(Companion.access$getTAG$p(companion));
    }
}
```

하지만 우리는 이런 불필요한 과정을 제거한 더 간단한 byte code를 얻을수 있습니다.

1. method call을 피하기 위해서 complile-time constant인 ```const```를 사용합니다.

	이렇게 하면 직접적으로 접근할 수 있지만 이것은 primitive type이나 String에서만 사용할 수 있습니다.

```
class MyClass {

    companion object {
        private const val TAG = "TAG"
    }

    fun helloWorld() {
        println(TAG)
    }
}
```

2. public field에 ```@JvmField``` annotation을 달아주면 getter나 setter를 생성하지 않고 순수 Java 상수처럼 동작하도록 할 수 있습니다. 하지만 이 annotation은 Java호환성을 위해서 만들어졌으므로 Java에서 상수 엑세스를 할 필요가 없는 경우 달아주지 않는 것이 좋습니다. 

	Android에서 예를 들면 ```Parcelable```을 구현하는데 주로 사용될 수 있습니다.

```
class MyClass() : Parcelable {

    companion object {
        @JvmField
        val CREATOR = creator { MyClass(it) }
    }

    private constructor(parcel: Parcel) : this()

    override fun writeToParcel(dest: Parcel, flags: Int) {}

    override fun describeContents() = 0
}
```

3. 마지막으로 ProGaurd 도구를 사용해서 byte code를 최적화를 통해서 chained method들을 병합하는 방법이 있지만 제대로 작동한다는 보장은 없습니다.

> companion object에서 static 상수를 읽으면 Java와 보다 kotlin이 상수 각각에 대해서 2-3 단계의 method가 추가됩니다.
> 
> primitive type이나 String을 사용할 땐 반드시 const키워드를 사용해야하고, 다른 유형의 타입은 상수를 반복적으로 엑세스 해야하는 경우 로컬 변수로 값을 캐시하여 회피할 수 있습니다.
>
>또 공용 전역 상수를 컴패니언 객체가 아닌 자체 객체의 퍼블릭 글로벌 상수로 선언해두는 것이 좋습니다.
