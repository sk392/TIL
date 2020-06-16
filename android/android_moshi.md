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
 : Adapter에서 제공해주는 fromJson / toJson에 대한 결과가 null을 허용해준다.
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



## 기본 사용 법.




## 요점

1. Enum엔 @JsonClass(generateAdapter = true)를 사용할 수 없다.
2. kotlin을 지원한다
3. Reflection은 런타임에 오버헤드가 있고, 난독화가 어렵다.
4. Code gen은 빌드 타임에 오버헤드가 있고 난독화할 수 있다.
5. Enum클래스를 참조하는 경우에는 (json to class) 반드시 대소 문자를 맞춰야 한다.

