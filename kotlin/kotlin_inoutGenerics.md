## kotlin_inoutGenerics

모던 랭귀지들은 타입바운드 개념을 제공합니다. 타입바운드는 3가지로 분류 할 수 있습니다. 무공변성(invariant), 공변성(covariant), 반공변성(contravariant), 3가지로 분류 할 수 있으며 하나씩 알아보겠습니다.

#### 무공변성(Invariant)

무공변성이란 상속 관계에 상관없이, 자기 타입만 허용하는 것을 말합니다.


```
object Main {
    @JvmStatic
    fun main(args: Array<String>) {

        val language: Some<Language> = object : Some<Language> {
            override fun something() {
                println("language")
            }

        }
        val jvm: Some<JVM> = object : Some<JVM> {
            override fun something() {
                println("jvm")
            }

        }
        val kotlin: Some<Kotlin> = object : Some<Kotlin> {
            override fun something() {
                println("kotlin")
            }

        }

        test(language)  //error
        test(jvm)       //ok
        test(kotlin)    //error

    }

    interface Some<T> {
        fun something()
    }

    fun test(value: Some<JVM>) {
    }

    open class Language

    open class JVM : Language()

    class Kotlin : JVM()
}
```


test라는 함수는 Some<JVM> 타입만을 입력받습니다. 따라서 language, kotlin은 허용하지 않습니다. 이런 개념을 무공변성이라고 합니다.

#### 공변성(covariant)

공변성은 타입생성자에게 리스코프 치환 법칙을 허용한다는 의미입니다. 코드로 살펴보겠습니다.
```
object Main {
    @JvmStatic
    fun main(args: Array<String>) {

        val language: Some<Language> = object : Some<Language> {
            override fun something() {
                println("language")
            }

        }
        val jvm: Some<JVM> = object : Some<JVM> {
            override fun something() {
                println("jvm")
            }

        }
        val kotlin: Some<Kotlin> = object : Some<Kotlin> {
            override fun something() {
                println("kotlin")
            }

        }

        test(language)  //error
        test(jvm)       //ok
        test(kotlin)    //ok

    }

    interface Some<T> {
        fun something()
    }

    fun test(value: Some<out JVM>) {
    }

    open class Language

    open class JVM : Language()

    class Kotlin : JVM()
}
```

34번째 라인을 보면 JVM 앞에 out 이라는 키워드가 추가되었습니다. out 키워드를 사용하면, 자기 자신과 자식 객체를 허용하겠다는 의미입니다. 따라서 무공변성에서 error가 발생했던 26번째 라인이 통과 되었습니다.

#### 반공변성(contravariant)

공변성의 반대 개념을 생각하면 쉽습니다. 자기 자신과 부모 객체만 허용하는 것을 말합니다.

```
object Main {
    @JvmStatic
    fun main(args: Array<String>) {

        val language: Some<Language> = object : Some<Language> {
            override fun something() {
                println("language")
            }

        }
        val jvm: Some<JVM> = object : Some<JVM> {
            override fun something() {
                println("jvm")
            }

        }
        val kotlin: Some<Kotlin> = object : Some<Kotlin> {
            override fun something() {
                println("kotlin")
            }

        }

        test(language)  //ok
        test(jvm)       //ok
        test(kotlin)    //error

    }

    interface Some<T> {
        fun something()
    }

    fun test(value: Some<in JVM>) {
    }

    open class Language

    open class JVM : Language()

    class Kotlin : JVM()
}
```

34번째 라인을 보면 in 이라는 키워드를 사용합니다. 그 결과로는 24번째 라인의 language와 jvm 은 컴파일이 되지만 JVM을 상속받은 kotlin 객체는 허용하지 않습니다.

### 정리
무공변성은 타입 앞에 아무런 키워드가 붙지 않으며, 해당타입 자신만을 허용합니다.

공변성은 out 키워드를 사용하며, 자기자신과 자기자신을 상속받은 타입을 허용합니다.

반공변성은 in 키워드를 사용하며, 자기자신과 자기자신의 슈퍼 타입을 허용합니다.