# Kotlin Singleton

Java -> kotlin으로 전환하던 와중에 싱글턴 형태에 대해서 의문이 들어 정리해본다.


### Pettern 1

getInstance()해서 호출하는 형태로 기존에 자바에서 많이 사용하던 패턴이다.
가장 익숙한 패턴이고 lazy하게 생성할 수 있도록 형태를 재가공했는데, 그러다보니 자바로 생성되는 코드들이 양이 많다.

```kotlin
class TempClass {
    companion object {
        private val INSTANCE : TempClass by lazy { TempClass() }

        @JvmStatic
        fun getInstance() = INSTANCE
    }
    fun getTempName()  : String{
        return "Temp"
    }

    fun getTempNumber() : Int{
        return 1
    }
}
```

```java
//Decompiled
public final class TempClass {
   private static final Lazy INSTANCE$delegate;
   public static final TempClass.Companion Companion = new TempClass.Companion((DefaultConstructorMarker)null);

   @NotNull
   public final String getTempName() {
      return "Temp";
   }

   public final int getTempNumber() {
      return 1;
   }

   static {
      INSTANCE$delegate = LazyKt.lazy((Function0)null.INSTANCE);
   }

   @JvmStatic
   @NotNull
   public static final TempClass getInstance() {
      return Companion.getInstance();
   }

   @Metadata(
      mv = {1, 1, 13},
      bv = {1, 0, 3},
      k = 1,
      d1 = {"\u0000\u0014\n\u0002\u0018\u0002\n\u0002\u0010\u0000\n\u0002\b\u0002\n\u0002\u0018\u0002\n\u0002\b\u0006\b\u0086\u0003\u0018\u00002\u00020\u0001B\u0007\b\u0002¢\u0006\u0002\u0010\u0002J\b\u0010\t\u001a\u00020\u0004H\u0007R\u001b\u0010\u0003\u001a\u00020\u00048BX\u0082\u0084\u0002¢\u0006\f\n\u0004\b\u0007\u0010\b\u001a\u0004\b\u0005\u0010\u0006¨\u0006\n"},
      d2 = {"LTempClass$Companion;", "", "()V", "INSTANCE", "LTempClass;", "getINSTANCE", "()LTempClass;", "INSTANCE$delegate", "Lkotlin/Lazy;", "getInstance", "LeetCodes"}
   )
   public static final class Companion {
      // $FF: synthetic field
      static final KProperty[] $$delegatedProperties = new KProperty[]{(KProperty)Reflection.property1(new PropertyReference1Impl(Reflection.getOrCreateKotlinClass(TempClass.Companion.class), "INSTANCE", "getINSTANCE()LTempClass;"))};

      private final TempClass getINSTANCE() {
         Lazy var1 = TempClass.INSTANCE$delegate;
         KProperty var3 = $$delegatedProperties[0];
         return (TempClass)var1.getValue();
      }

      @JvmStatic
      @NotNull
      public final TempClass getInstance() {
         return ((TempClass.Companion)this).getINSTANCE();
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


### Pettern 2

object를 사용한는 형태로 kotlin에서 사용하라고 [권장](https://kotlinlang.org/docs/reference/object-declarations.html#object-expressions-and-declarations)하는 방법이다.

```kotlin
object TempClass {
    var tempCount = 0
    @JvmStatic
    fun getTempName()  : String{
        return "Temp"
    }

    @JvmStatic
    fun getTempNumber() : Int{
        tempCount++
        return 1
    }
}

```

```java
//Decompiled

public final class TempClass {
   private static int tempCount;
   public static final TempClass INSTANCE;

   public final int getTempCount() {
      return tempCount;
   }

   public final void setTempCount(int var1) {
      tempCount = var1;
   }

   @JvmStatic
   @NotNull
   public static final String getTempName() {
      return "Temp";
   }

   @JvmStatic
   public static final int getTempNumber() {
      int var10000 = tempCount++;
      return 1;
   }

   private TempClass() {
   }

   static {
      TempClass var0 = new TempClass();
      INSTANCE = var0;
   }
}


``` 


# 뭐가 더 좋아?

음.. 결론 부터 얘기하면 Object가  미세하지만 더 좋다.

getInstance형태와 마찬가지로 object도 [lazy init](https://kotlinlang.org/docs/reference/object-declarations.html#semantic-difference-between-object-expressions-and-declarations)을 하기 때문에 메모리 상의 차이점은 없고, getInstnace형태가 코드 생성되는 양도 많고, 불필요한 코드이며 단순히 getInstance형태가 익숙하다는 것 뿐이다.

다만 getInstance를 사용할 때 이점은 현재는 Java와 kotlin 코드가 섞여있어서 매 메서드마다 @JvmStatic을 붙여야하니 귀찮고, getInstance -> object로 바꾸려면 다른 클래스들에 건들이는 것들도 많아지니 컨플릭을 최소화 하기 위해서 일단 둘다 용인하는 형태로 고고!