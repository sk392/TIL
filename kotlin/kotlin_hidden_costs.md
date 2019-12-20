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
 

```
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

```
val deletedRows = transaction(db) {
    it.delete("Customers", null, null)
}
```

java6 JVM에서와 같이 람다식을 지원하지 않는 곳에서는 어떤식으로 바이트 코드가 생성될까요? 예상한대로 람다 및 익명함수는 function객체로 컴파일이되는걸 확인할 수 있습니다.

### Function Object

 컴파일 후에 람다식의 표현은 아래와 같습니다
 
 ```
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

```
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

```
db.beginTransaction();
try {
   int result$iv = db.delete("Customers", null, null);
   db.setTransactionSuccessful();
} finally {
   db.endTransaction();
}
```
 
 하지만 이 기능에도 몇 가지 조심해야하는 사항이 있습니다.
 
 * inline함수는 자기자신이나 다른 inline함수를 부를 수 없습니다.
 * public으로 선언된 inline함수는 해당 클래스의 public함수나 public필드에만 접근할 수 있습니다.
 * inline으로 구현하면 코드가 직접 들어가기 때문에 절대적인 코드 크기는 커집니다. 그리고 또한 이 inline에 속한 함수가 코드가 긴 함수라면 코드 길이는 더더욱 늘어날 수 있습니다.

 