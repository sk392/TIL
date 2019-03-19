## Kotlin의 실행 순서

### Constructor 와 init

코틀린에선 초기화할 때 constructor (생성자)를 이용하거나 init()을 사용하여 객체가 생성될때 필요한 초기화 작업을 할 수 있다.


### 객체 생성 시 초기화


#### Property initalizers

 가장 기본적인 프로퍼티 선언과 동시에 초기화 할 수 있다. function을 콜하거나 by를 통해 delegation도 가능!
 
```
val count : Int = 0

val data = getData()
```

#### Initialize blocks

 보통 객체 상단에 넣으며 여러 개를 넣어도 모두 호출된다.
 
```
init{
 //something
}

```


#### Constructor

 생성자 역시 class의 객체 생성시 호출된다.

class정의와 함께 붙는 primary constructor가 있으며, parameter에 따라 달라지는 secondary constructor를 여러개 만들수도 있다.
 
```
class Person(val name: String ) { //primary constructor
  constructor (age: Int, address: String) { 
  //do something 
  }
  constructor (age: Int, address: String, company: String) {
    //do something
  } 
   
}
```


### Execution order

호출되는 순서는 다음과 같다, 특이하게도 생성자 내부 블럭 코드가 가장 나중에 수행된다는 점을 유의하면 된다.

1. 호출한 생성자의 arguments에 대해 수행
2. 만약 해당 생성자에서 다른 생성자를 호출하고 있다면, 호출된 생성자의 arguments가 초기화
3. 상단에서부터 차례대로 property와 init 블록들이 초기화
4. 생성자 블럭 내부의 코드들이 수행


```
//Example Code

open class Parent {
    private val a = println("Parent.a - #4")

    constructor(arg: Unit = println("Parent primary constructor default argument - #3")) {
        println("Parent primary constructor")
    }

    init {
        println("Parent.init - #5")
    }

    private val b = println("Parent.b - #6")
}

class Child : Parent {
    val a = println("Child.a - #7")

    init {
        println("Child.init 1 - #8")
    }

    constructor(arg: Unit = println("Child primary constructor default argument - #2")) : super() {
        println("Child primary constructor")
    }

    val b = println("Child.b - #9")

    constructor(arg: Int, arg2: Unit = println("Child secondary constructor default argument - #1")) : this() {
        println("Child secondary constructor - #11")
    }

    init {
        println("Child.init 2 - #10")
    }
}

```

출력 순서는 다음과 같다.

```

Child secondary constructor default argument - #1

Child primary constructor default argument - #2

Parent primary constructor default argument - #3

Parent.a - #4

Parent.init - #5

Parent.b - #6

Parent primary constructor - #7

Child.a - #8

Child.init 1 - #9

Child.b - #10

Child.init 2 - #11

Child primary constructor - #12

Child secondary constructor - #13

```