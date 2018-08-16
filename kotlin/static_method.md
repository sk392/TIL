## Java의 Static methods -> kotlin으로 바꿀 때 

Java 와 Kotlin 혼합사용시 Utils 형태의 Static Methods 를 바꿀때. 사용하는 방법에 대한 정리입니다.

* 좀 더 Kotlin 스럽게
* 실제 .class 코드가 복잡하지 않도록

Reference

* https://kotlinlang.org/docs/reference/java-to-kotlin-interop.html#static-methods
## 최초 코드

Utils 클래스 TUtils 과 해당 클래스를 사용하는 TUse 가 있습니다.

```
class TUtils {
    public static void method1() {
        System.out.println("Print");
    }
}
```

```
class TUse {
    public static void main(String[] args) {
        TUtils.method1();
    }
}
```

TUse.java는 java 파일로 유지한채 TUtils 클래스를 Kotlin으로 변환할때 다음과 방법을 많이 사용합니다.

### 1. TUse를 object Class로 변환
기본적인 Convert Java to Kotlin File 사용시 변환되는 형태입니다.

```
object TUtils {
    fun method1() {
        println("Print")
    }
}
```

```
class TUse {
    public static void main(String[] args) {
        TUtils.INSTANCE.method1();
    }
}
```

object 지정자를 가진 TUtils 클래스는 싱글톤이 되며, TUse 에서는 해당 싱글톤 인스턴스를 사용하는 형태가 됩니다.

### 2. TUse를 일반 class로 한 후 companion object 처리

```
class TUtils {
    companion object {
        fun method1() {
            println("Print")
        }
    }
}
```

```
class TUse {
    public static void main(String[] args) {
        TUtils.Companion.method1();
    }
}
```

이 형태는 kotlin에서 companion object 을 이용해서 외부에서 static 으로 접근가능하게 되었습니다.

하지만, TUse 에서 아래와 같은이, {class명}.Companion.{method명} 형태로 사용되고 있네요.

```
TUtils.Companion.method1();
```

기존 java 코드가 바뀌는 형태다보니, @JvmStatic을 이용해서 java에서 해당 메소드를 static 하게 접근하도록 변경가능합니다.

```
class TUtils {
    companion object {
        @JvmStatic
        fun method1() {
            println("Print")
        }
    }
}
```

```
class TUse {
    public static void main(String[] args) {
        TUtils.method1();
    }
}
```

## Kotlin 스럽게 하자.

Kotlin에는 static method 개념이 없습니다. 대신 package level method 가 있지요. 그 이것을 이용하도록 하겠습니다.

```
fun method1() {
    println("Print")
}
```

```
class TUse {
    public static void main(String[] args) {
        TUtilsKt.method1();
    }
}
```
Kotlin 코드가 package level method로 변경되었고, java에서도 접근가능하지만, TUtils.kt 은 Kotlin Compiler에 의해 TUtilsKt가 됩니다.

하지만, java 쪽 코드가 바뀌었네요... 우리들이 원하는 형태는 TUtils 입니다. @file:JvmName(name: String)을 이용해 원하는 명칭으로 수정이 가능합니다.

```
@file:JvmName("TUtils")

fun method1() {
    println("Print")
}
```

```
class TUse {
    public static void main(String[] args) {
        TUtils.method1();
    }
}
```

1. Kotlin 파일의 class를 지웁니다.
2. @file:JvmName("TUtils") 을 추가 해줍니다.

이로써, 자바 파일의 변화없이 Kotlin 스럽게 Convert 했습니다.

``` @file:JvmName(name: String)```  을 사용함으로써 원하는 형태로 이름이 생성되었습니다.

최종 형태의 Kotlin 코드를 Kotlin Byte -> Decompile 했을때 아래처럼 됩니다.

```
package aa;

import kotlin.Metadata;
import kotlin.jvm.JvmName;

@Metadata(
   mv = {1, 1, 7},
   bv = {1, 0, 2},
   k = 2,
   d1 = {"\u0000\b\n\u0000\n\u0002\u0010\u0002\n\u0000\u001a\u0006\u0010\u0000\u001a\u00020\u0001¨\u0006\u0002"},
   d2 = {"method1", "", "production sources for module untitled"}
)
@JvmName(
   name = "TUtils"
)
public final class TUtils {
   public static final void method1() {
      String var0 = "Print";
      System.out.println(var0);
   }
}
```

최초 코드와 거의 흡사하죠?

## 결론

Android 혹은 Java와 함께 Kotlin 사용시 Java와의 호환은 필수적인 부분이지만, Java를 위한 Kotlin 코드를 작성하는지 생각해볼 필요가 있을것 같습니다.

이번 어프로치는 Kotlin 스럽게 작성하고 특정 Annotation 들만 제거하는 진행방식이 Kotlin-Java 혼합코드에서 Kotlin 100% 코드로 갔을때 에러를 줄일 수 있는 방법이 아닐까 생각해서 해본 방법입니다.

