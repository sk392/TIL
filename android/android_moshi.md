# Moshi 적용 검토하기.

Moshi를 검토하게 된 이유가 Gson을 잘 쓰다가 kotlin도 지원되고, 좋다는 얘기를 많이 들어서 한번 검토해보려고 간을 보고 있는 와중에 Gson에서 IncompatibleClassChangeError라는 익셉션이 발생했는데 Reflection을 사용해서 발생한거라 code gen 을 활용하는 Moshi로 이동해보려고 [Moshi](https://github.com/square/moshi) 와[Gson](https://github.com/google/gson)의 특징과 차이를 정리해봐야겠다.


## Moshi

### Built-in Type Adapters

모쉬는 Build-in으로 java의 중요 기능들을 읽거나 쓸 수 있게 도와준다.

* Primitives (int, float, char...) and their boxed counterparts (Integer, Float, Character...)
* Arrays, Collections, Lists, Sets, and Maps
* Strings
* Enums

### Custom Type Adapter

또 모쉬의 다른 특징으로는 함수에 ```@ToJson``` , ```@FromJson```을 지정해서  원하는 클래스의 Adpter를 원하는대로 구성할 수 있다.

```java
class CardAdapter {
  @ToJson String toJson(Card card) {
    return card.rank + card.suit.name().substring(0, 1);
  }

  @FromJson Card fromJson(String card) {
    if (card.length() != 2) throw new JsonDataException("Unknown card: " + card);

    char rank = card.charAt(0);
    switch (card.charAt(1)) {
      case 'C': return new Card(rank, Suit.CLUBS);
      case 'D': return new Card(rank, Suit.DIAMONDS);
      case 'H': return new Card(rank, Suit.HEARTS);
      case 'S': return new Card(rank, Suit.SPADES);
      default: throw new JsonDataException("unknown suit: " + card);
    }
  }
}
```

```java
Moshi moshi = new Moshi.Builder()
    .add(new CardAdapter())
    .build();
```

https://gist.github.com/swankjesse/61354fd0a20bf56072f6a1d0c82fb9fc



### Adapter convenience methods

Moshi에 Adapter에서는 또 편의성을 위한 함수들이 여러가지가 존재하는데 리스트는 아래와 같다.

* nullSafe()
 : Adapter에서 제공해주는 fromJson / toJson에 대한 결과가 null을 허용해준다. 기본적으로 nullsafe이다.
 ```val item = itemAdapter.nullSafe().toJson(null) // result is null ```
 ```val item = itemAdapter.nullSafe().toJson(null) // result is null ```
 
* nonNull()
 : Adapter에서 제공해주는 fromJson / toJson에 대한 결과가 null을 허용하지 않는다.
 ```val item = itemAdapter.nullSafe().toJson(null) // exception ```
 
* lenient()
 : Json의 형식을 느슨하게 바꿔줍니다. 기존은  [여기](https://www.ietf.org/rfc/rfc7159.txt)에 서술되어 있는 기본만 적용되지만 lenient로 사용할 경우 모든 타입의 최상위 값을 사용할 수 있고(기본은 반드시 객체거나 배열이어야함) 숫자는 Double에 NaN이나 Infinite도 허용됩니다.
 
* failOnUnknown()
 : 아래와 같은 코드가 있을때 기본적으론 알 수 없는 변수가 나오면 SkipName이나 SkipValue로 넘어가는데, 요걸 사용하면 exception이 떨어진다. ```adatper.fromJson(testJson)``` 을 호출하면 null이 떨어지고  ``` adatper.failOnUnknown().fromJson(testJson)```를 호출하면 exception이 떨어진다.

```kotlin
@JsonClass(generateAdapter = true)
data class Item(val id: String?, val id2: String?)

val testJson = """{
    "id3": "null"
    }
    """
```

* indent()
 : ```adapter.toJson()```을 호출할 때 읽기 쉽게 indent를 추가해준다.
 
* serializeNulls()
 :  ```adapter.toJson()```호출할 때 null도 직렬화가 됩니다.

### Fails Gracefully

 일반적으로 Reflection과 달리 Moshi는 일이 잘못 될 때 도움을주기 위해 고안되어 JSON 문서를 읽는 중 오류가 발생하거나 형식이 잘못된 경우 java.io.IOException을 발생시키고, 타입 포맷과 일치하지 않으면 JsonDataException이 발생합니다.
 
### Custom field name

Moshi는 @Json(name = "name")을 활용해서 json에 이름과 변수의 이름을 다르게 설정하고 매핑할 수 있습니다. ```@Json(name = "closed_issues") val closedCount: Long``` 


### @JsonQualifier

커스텀 어댑터를 사용할때 같은 타입의 값에 대한 특별한 처리를 하고 싶을 때 @JsonQualifier를 활용하시면 된다. (e.g. color값을 int로 받기)

아래와 같이 colorItem에 다같은 int값인데 다르게 처리하고 싶을 때 CustomAdapter를 만들어서 Moshi빌드할 때 넣어주고 adapter를 빼면 된다. 여기서 헷갈릴 수 있는게 CustomAdapter가 ColorItem에 대한 encoding이나 decoding을 해주는 것이 아니고 하나의 어노테이션에 대해서만 특별하게 처리해준다.


```kotlin


@JsonClass(generateAdapter = true)
data class ColorItem(val width : Int, val height : Int, @HexColor val color : Int)

class CustomAdapter{
    @ToJson
    fun toJson(@HexColor rgb: Int): String? {
        return String.format("#%06x", rgb)
    }

    @FromJson
    @HexColor
    fun fromJson(rgb: String): Int {
        return rgb.substring(1).toInt(16)
    }
}
```

```kotlin
//사용 예시
val moshi = Moshi.Builder().add(CustomAdapter()).build()
val itemAdapter = moshi.adapter(ColorItem::class.java)
val item = itemAdapter.fromJson(colorJson)
```


### 생략하고 싶은 필드가 있다면 사용해보아요 @transient

특정 모델들은 데이터를 json에 포함시키고 싶지 않은 값들이 있을 수 있습니다 그럴 때 ```@Transient```를 활용한다면 해결할 수 있어요! fromeJson이나 toJson할 때 해당 값을 Skip하게되고 만약 ```@Trasient``` 어노테이션이 달렸음에도 불구하고 Json에 해당 값이 있다면(원랜 toJson할때 스킵되서 없는게 정상) FailOnUnknown로 adapter로 부르면 크래시가 난다. 모델에 데이터를 채워줄 때는 fromJson할때는 기본 값을 얻습니다. 그래서 ```@Trasient```가 달렸다면 반드시 default Value를 선언해줘야한다.


## 기본 사용 법.

kotlin에서의 사용 방법은 크게 2가지로 [Reflection](https://github.com/square/moshi#reflection)을 사용하는 방법과 [Code generation](https://github.com/square/moshi#reflection)을 활용 하는 방법이 있는데, 그 중에 Code Gen방식으로 사용 방법을 알아보자.

1. 우선 Code Gen을 활용하기 위해서 Gradle에 디펜던시를 추가해준다. 

```
implementation("com.squareup.moshi:moshi:1.9.3") // moshi
kapt("com.squareup.moshi:moshi-kotlin-codegen:1.9.3") // for code-gen
```

2. toJson / fromJson을 원하는 클래스에 ```@JsonClass(generateAdapter = true)```를 넣어주어 adapter가 제너레이트 되도록 한다. (작성하지 않으면 기본 reflection인데 관련 디펜던시가 없으면 크래시가 날 것이다.)

```kotlin

@JsonClass(generateAdapter = true)
data class Item(val id: String?, val id2: String?)
```

3. 특정 파라미터 이름을 다르게 하고 싶을 떈 ```@Json(name = "name")```를 달아서 변경한다 (SerializedName과 같은 기능)

4. Moshi를 만들고 특정 파라미터에 대한 원하는 처리를 하고 싶다면 ```@JsonQualifier ``` 를 활용해서 Custom Annotation을 만들고 해당 Adapter를 MoshiBuilder에 직접 넣어준다.

```kotlin
//create Moshi
    private val moshi: Moshi =
        Moshi.Builder()
            .build()
```

5. 특정 클래스에 대한 커스텀 어뎁터가 필요한 경우 Adapter를 만들고 Moshi에 넣어준다.

```
class CardAdapter {
    @ToJson
    fun toJson(card: Card): String {
        return card.rank + card.suit.name().substring(0, 1)
    }

    @FromJson
    fun fromJson(card: String): Card {
        if (card.length != 2) throw JsonDataException("Unknown card: $card")
        val rank = card[0]
        return when (card[1]) {
            'C' -> javax.smartcardio.Card(rank, Suit.CLUBS)
            'D' -> javax.smartcardio.Card(rank, Suit.DIAMONDS)
            'H' -> javax.smartcardio.Card(rank, Suit.HEARTS)
            'S' -> javax.smartcardio.Card(rank, Suit.SPADES)
            else -> throw JsonDataException("unknown suit: $card")
        }
    }
}

```
```kotlin
//create Moshi
    private val moshi: Moshi =
        Moshi.Builder()
            .add(CardAdapter())
            .build()
```

6. retrofit에서 사용하기 위해서 제공해주는 MoshiConverterFactory를 넣어준다

```
	 Retrofit.Builder()
            .addConverterFactory(MoshiConverterFactory.create())
		...
```


## Migration from Gson (feat. kotlin)

1. Gson은 Json으로 전환할 때 컨스트럭터에 있는 값만 convert해주고, Moshi는 내부에 있는 멤버변수까지 컨버팅해준다.
2. 1.번에 연장인데, Moshi는 private 멤버변수를 convert해주지 못한다. (code generation)
3. 멤버 변수일 때  var일때 Moshi는 toJson에 들어가며 val일 때는 안들어가는데, gson은 무조건 안들어간다.
4. JsonObject -> mutalbeMap , map, HashMap, 어떤 객체를 사용하던, Type은 Map::class.java를 넣어줘야한다.
5. 4번과 마찬가지로 JsonArray는 -> List로 변환해주면 된다.
6. private 변수는 컨버팅이 안되므로 @Transient를 달아줘야 하는데, get() / set()을 오버라이드 한 경우면 불가능하다.
7. private 하고 싶으면  @Trasient 달아야하는데 get() / set()오버라이드 된 경운 안달린다.
8. primary type의 경우에 값이 내려오지 않는 경우 널러블로 바꿔주거나, default값을 내려줘야 한다.
9. nullable에 값이 내려오지 않으면 null이다
10. Moshi를 통해 Retrofit에 Generic을 사용할 경우 결과 타입에서 형태를 정해서 사용하거나 아니면 사용하지 않아야 한다. 런타임중에 알 수 없기 때문
11.  codeGen타입은 리플렉션이 아니기 때문에 private으로 된 멤버 변수는 사용할 수 없다. @Trasient 어노테이션을 달거나 함수로 빼야하는데, @Trasient어노테이션을 달기 위해선 get() / set()에 대한 오버라이드가 있는 경우 사용할 수 없어서 함수로 대체. 
12. Generic이 포함된 상속 타입에서는 retrofit에서 찾아줄 수 없다 (
13. JsonObject to Map with @JvmSuppressWildcards

```kotlin
class A() : B<C>(){

}
open class B<T> {

}
//Not
fun getApiResult() : Single<A>

//correct
fun getApiResult() : Single<B<C>>
```



## 기타사항 정리

1. Enum엔 @JsonClass(generateAdapter = true)를 사용할 수 없다.
2. Reflection은 런타임에 오버헤드가 있고, 난독화가 어렵다.
3. Code gen은 빌드 타임에 오버헤드가 있고 난독화할 수 있다.
4. Enum클래스를 참조하는 경우에는 (json to class) 반드시 대소 문자를 맞춰야 한다.
5. Constructor에 있지 않은 변수는 toJson, fromJson에서 유의미하지 않다.
6. get(),set() override는 컨버팅하는데 이상이 없다.
7. val일경우 toJson할 때 사용하지 않는다.
8. private val / pirvate set 등다 안된다.



## 대체 가능한지 고려사항

1. private 변수는 toJson / fromJson을 사용할 수 없음. @Trasient를 달아주면 되는데 결국 json convert는 불가능.
2. 제네릭 타입 사용 제한 retrofit에서는 

