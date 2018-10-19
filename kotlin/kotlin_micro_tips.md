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