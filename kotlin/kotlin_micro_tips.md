# Kotlin Micro Tips

## 기초 함수

#### let, also, apply, run, ?

코틀린에서는 기본 확장 함수들이 있는데, 예를들어 apply는 해당 표현식에 대한 오브젝트를 참조하고, 또 반환도 오브젝트로 이루어 진다. 

```
val textView = findViewById<TextView>(R.id.textView)

textView?.apply{
	text = "블라블라"
}
```

위와 같이 textView를 스마트 캐스팅으로 가져올 때 선언된 textView는 nullable형태로 가져오게 되는데, 이를 해결하기 위해서 보통 apply를 쓰게 된다.

또 내부 text 세팅을 위해 apply를 쓰면 불필요한 func콜이 늘어나게된다.apply 내부 코드는 다음과 같은데 (다른 let,also,run 비슷하다)

```
public inline fun <T> T.apply(block: T.() -> Unit): T {
    contract {
        callsInPlace(block, InvocationKind.EXACTLY_ONCE)
    }
    block()
    return this
}
```

내부적으로 callsInPlace 함수를 콜하고 들어온 함수를 한번 더 콜하게 된다. 성능에 큰 이슈는 아니지만 불필요한 함수 콜을 한번 하는 것이므로 다음과 같이 짜는 게 좋다.

```
val textView :TextView = findViewById(R.id.textView)

textView.text = "블라블라"

```

위와같이 수정한 경우 TextView를 가져올 때 스마트 캐스팅을 피하고, 불필요한 함수 콜을 줄여서 성능 향상을 꾀할 수 있다. (큰 영향은 아니라도..)


#### let, also, apply, run 구분하기!

kotlin을 사용하다보면 비슷비슷한데 구분하기 애매한 것들이 있다. 나는 그나마 구분하는게 apply, let 요 2가진데, apply는 해당 object를 초기화 하거나 세팅할 때, let은 단순이 nullable을 때기 위해서 사용하는데, 나머진 좀 아리까리해서 정리해 보았다.


![간편도표](https://i.stack.imgur.com/aLUv7.png)

간단하게 표현하자면 아래와 같은 클래스가 있을 때

```
class MyClass {
    fun test() {
        val str: String = "..."
        val result = str.xxx {
            print(this) // Receiver
            print(it) // Argument
            42 // Block return value
        }
    }
}
```

반환결과는 아래와 같다. 자세한 설명은 내일 커밋한다!

```
╔══════════╦═════════════════╦═══════════════╦═══════════════╗
║ Function ║ Receiver (this) ║ Argument (it) ║    Result     ║
╠══════════╬═════════════════╬═══════════════╬═══════════════╣
║ let      ║ this@MyClass    ║ String("...") ║ Int(42)       ║
║ run      ║ String("...")   ║ N\A           ║ Int(42)       ║
║ run*     ║ this@MyClass    ║ N\A           ║ Int(42)       ║
║ with*    ║ String("...")   ║ N\A           ║ Int(42)       ║
║ apply    ║ String("...")   ║ N\A           ║ String("...") ║
║ also     ║ this@MyClass    ║ String("...") ║ String("...") ║
╚══════════╩═════════════════╩═══════════════╩═══════════════╝
```


다음편에서 더 다뤄봐야겠다 난 퇴근이하고 시픙니까!

