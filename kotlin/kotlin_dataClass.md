## Kotlin DataClass

며칠 안하다보니 계속안할 것같아서 머라도 간단하게한다!


#### Destructuring declarations

이 data키워드는 소멸 선언 을 허용하는 함수를 제공합니다 . 즉, 모든 속성에 대해 함수를 생성하므로 다음과 같은 작업을 수행 할 수 있습니다.

```
val steveJobs= User("Steve Jobs", 56)

fun print() {
  val (name, age) = steveJobs
  println("$name, $age years of age") // prints "Steve Jobs, 56 years of age"

  steveJobs.component1() // name
  steveJobs.component2() // age
}
```

#### Copy function

이 data키워드는 클래스의 일부 속성 값을 변경하는 편리한 복사 방법을 제공합니다. 연령을 변경하는 사용자의 복사본을 만들고 싶다면 다음과 같이하십시오.

```
val steveJobs= User("Steve Jobs", 56)
val steveJobsToday= steveJobs.copy(age = 63)
```

#### Properties declared in the class body are ignored

컴파일러는 자동 생성 함수에 대해 기본 생성자 내부에서 정의 된 속성 만 사용합니다.

```
data class User(val name: String, val age: Int) {
  var address : String = ""
}
```



### 요구 사항 및 제한 사항

* 데이터 클래스 생성자에는 하나 이상의 매개 변수가 있어야합니다.
* 모든 매개 변수는 val또는 로 시장이되어야 var 합니다.
* 데이터 클래스는 또는 abstract일 수 없습니다  .opensealedinner equals , toString및 hashCode방법은 명시 적으로 재정의 할 수 있습니다.
* 명시 적 구현 componentN()및 copy()함수는 허용되지 않습니다. copy()서명 과 일치 하는 함수 가있는 형식에서 데이터 클래스를 파생시키는 것은 Kotlin 1.2에서 더 이상 사용되지 않으며 Kotlin 1.3에서는 금지되었습니다.
* data클래스는 서로 확장 할 수 없습니다 data클래스입니다.
* data클래스가 다른 클래스를 확장 할 수있다