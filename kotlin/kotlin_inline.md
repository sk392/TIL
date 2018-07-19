## inline function

#### 구현 유무 그리고 인라인 여부에 따른 함수의 동작 확인

```kotlin
fun filledFunction() {
    print("I'm full!!")
}

inline fun filledInlineFunction() {
    print("I'm full and also inlined!!")
}

fun emptyFunction() {
    // Yay, no content here!
}

inline fun emptyInlineFunction() {
    // Yay, no content here!
}

fun test1() {
    filledFunction()
}
fun test2() {
    filledInlineFunction()
}
fun test3() {
    emptyFunction()
}
fun test4() {
    emptyInlineFunction()
}
```

```java

// -> Decompile

public final class InlineKt {
   public static final void filledFunction() {
      String var0 = "I'm full!!";
      System.out.print(var0);
   }

   public static final void filledInlineFunction() {
      String var1 = "I'm full and also inlined!!";
      System.out.print(var1);
   }

   public static final void emptyFunction() {
   }

   public static final void emptyInlineFunction() {
   }

   public static final void test1() {
      filledFunction();
   }

   public static final void test2() {
      String var0 = "I'm full and also inlined!!";
      System.out.print(var0);
   }

   public static final void test3() {
      emptyFunction();
   }

   public static final void test4() {
   }
}
```

오, 역시 inline은 호출부에 직접 코드를 생성해주므로, 비어있는 인라인 함수는 생성 코드에 영향을 미치지 않는다.

## 어디에 쓸까?

#### 로그

```java

// 1. 일반적인 로그 출력
Log.v(TAG, "Received JSON: " + JSON.toString());

// 1의 경우는 무조건 출력이 되므로, 모드에 따라 전환하고 싶은 경우가 있다.
// 2. 이런 경우 아래와 같이 많이 처리한다.
if (Env.isDebug()) {
    Log.v(TAG, "Received JSON: " + JSON.toString());
}

// 2번의 경우도 타이핑이 귀찮은 관계로 
// 3. 내부에서 조건을 처리하거나 실제 로그의 구현체를 분리하는 Log의 Wrapper를 정의해서 쓰는 경우도 있다.
ConditionalLog.v(TAG, "Received JSON: " + JSON.toString());
```

사실 3번의 경우도 문제는 있는게 실제 로그 출력을 하지 않더라도 JSON.toString()이나 String concat 연산은 무조건 발생한다. 이런 것은 좋지 않다. 굳이 해야 한다면 2번이 나을지도 모른다.

##### 만약, 개발 시에만 사용하는 코드라면???

런타임에 조건에 따라 로그를 출력하는 경우라면 사실 DI 형태를 차용하면 되므로, Class delegation을 응용하여, Logger를 상태에 따라 출력하고 이를 전달해서 사용해도 된다. (개인적으로도 이 방법이 좋다고 생각한다.)

그렇다면 함수의 구현이 비어있을 경우 구현으로 치환하는 inline의 특성 상 로그 코드를 아예 삭제하는 것이 가능할 것 같다. 예를 들어 다음과 같은 코드가 있다고 치면...

```kotlin
fun doSomething() {
    print("I'm doing something!")
    trace("doSomething() is doing something!")
}
```

디버깅 빌드 시에는 아래와 같이...

```kotlin
inline fun trace(message: String) {
    print(message)
}
```

릴리즈 시에는 아래와 같은 코드로 효율적인 처리가 가능하다.

```kotlin
inline fun trace(message: String) {}
```

#### 이슈: unused variable의 잔존

```kotlin
inline fun trace(message: String) {}

fun doSomething() {
    print("I'm doing something!")
    trace("doSomething() is doing something!")
}
위 코드를 디컴파일해보면...

public final class InlineKt {
   public static final void trace(@NotNull String message) {
      Intrinsics.checkParameterIsNotNull(message, "message");
   }

   public static final void doSomething() {
      String var0 = "I'm doing something!";
      System.out.print(var0);
      var0 = "doSomething() is doing something!";
   }
}
```

로 var0라는 unused var이 존재한다. 보통은 elimination을 할텐데 왜 남아있을까.