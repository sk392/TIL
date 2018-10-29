# Kotlin Micro Tips

## 기초 함수

#### let, also, apply, run, with, ?

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



### let, also, apply, run, with 구분하기!

kotlin을 사용하다보면 비슷비슷한데 구분하기 애매한 것들이 있다. 나는 그나마 구분하는게 apply, let 요 2가진데, apply는 해당 object를 초기화 하거나 세팅할 때, let은 단순이 nullable을 때기 위해서 사용하는데, 나머진 좀 아리까리해서 정리해 보았다.


![간편도표](https://i.stack.imgur.com/aLUv7.png)

#### - let()

정의 : 함수를 호출하는 객체가 이어지는 블록의 인자로 넘기고 결과 값을 반환

사용 예시 : 함수를 호출한 객체를 인자로 받으므로, 이를 사용하여 다른 메서드를 실행하거나 연산을 수행해야하는 경우 사용

```
 setOnClickListener{
	 item.link?.let{
	 	openLink(link)
	 }
 }
```


#### - apply()

정의 : 는 함수를 호출하는 객체를 이어지는 블록의 리시버 로 전달하고, 객체 자체를 반환

리시버란, 바로 이어지는 블록 내에서 메서드 및 속성에 바로 접근할 수 있도록 할 객체를 의미

사용 예시 : 특정 객체를 생성하면서 함께 호출해야 하는 초기화 코드가 있는 경우 사용할 수 있다. 


```
val textView = findViewById<TextView>(R.id.textView)

textView?.apply{
	text = "블라블라"
	setTextColor(R.color.red)
	}
```

#### - run()

정의 : 함수는 인자가 없는 익명 함수처럼 동작하는 형태와 객체에서 호출하는 형태 총 두 가지가 있다.

객체 없이 run() 함수를 사용하면 인자 없는 익명 함수처럼 사용할 수 있다. 이어지는 블럭 내에서 처리할 작업들을 넣어줄 수 있으며, 일반 함수와 마찬가지로 값을 반환하지 않거나 특정 값을 반환할 수도 있다.

사용 예시 : 객체에서 이 함수를 호출하는 경우 객체를 리시버로 전달받으므로, 특정 객체의 메서드나 필드를 연속적으로 호출하거나 값을 할당할 때 사용한다. 주의할 점은 apply랑 비슷하지만, apply는 새로운객체를 생성함과 동시에 연속된 작업을 할 때사용하고 run은 이미 생성된 객체에 연속된 작업을 할 때 필요하다.

```

link?.run { url.isNotBlankOrNull() || androidScheme.isNotBlankOrNull() } == true
```

#### - with()

정의 : 함수를 호출하는 객체가 이어지는 블록의 인자로 넘기고 결과 값을 반환한다, 사실상 with()함수는 run()함수와 거의 동일하며 리시버로 전달할 객체가 어디에 위치했는지만 다르다. 

run()함수는 with()함수를 좀 더 편리하게 사용하기 위해 let()함수와 with()함수를 합쳐놓은 형태로 봐도 무방하다!!

주의할 점은 with()함수는 Safe Calls을 지원하지 않고 run()은 지원한 점을 인지해야 된다!

사용 예시 : Non-nullable (Null 이 될수 없는) 수신 객체 이고 결과가 필요하지 않은 경우에만 with 를 사용한다!

```
 val configuration = Configuration() 
    with(configuration) {
        host = "127.0.0.1"
        port = 9000            
        isSSL = true
    }
```

#### - also()

정의 : 확장함수로 수신 객체를 암시적으로 받고, 매개변수 T로 명시적으로 코드 블럭 내로 전달하게 된다. 또 also는 코드 블록 내에 전달된 수신객체를 그대로 반환한다.

사용 예시 : 수신 객체 람다가 전달된 수신 객체를 전혀 사용 하지 않거나 수신 객체의 속성을 변경하지 않고 사용하는 경우 also 를 사용한다. 예를 들면 유효성 검사를 할 때 아주 유용하다!

```
 extraInfoView.visibility = extraInfoItem.visibility.also {
                if (it == View.VISIBLE) {
                    extraInfoView.layoutParams.width = viewSize.imageWidth
                    extraInfoView.setExtraInfos(extraInfoItem)
                    applyThemeToExtraInfo(extraInfoView, extraInfoItem)
                }
            }
```

#### 코드로 보는 간단한 정리


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

