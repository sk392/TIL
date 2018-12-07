## Kotlin에서의 String


Kotlin의 String에 대해서 작은 의문과 그 것을 이해함을 기반으로 정리해본다. 우선, Kotlin([나무위키](https://namu.wiki/w/Kotlin)) 은 JVM기반의 언어이며, Java와의 호환을 100퍼센트 지원된다. (하지만 컨버팅을 완벽하게 해주는것은 아니다;)

우선 String은 프리미티브 타입이 아니므로 오브젝트를 가지게 된다. 그래서 자바에서는 '==' 과 '.equals()'를 구분해서 사용하고 있었는데, 

```
String temp = "Temp";
String temp2 = "Temp";

System.out.println(temp.equals(temp2)); // true
System.out.println(temp == temp2); // true
```

이렇게 둘다 트루가 나온다. 그 이유는, Jvm에서 String을 저장하는 특수 메모리 영역인 Java String Pool에 저장하고 별도로 오브젝트를 만들지 않기 때문이다. 별도로 생성자를 통해 만들어진 경우는 새로 오브젝트를 생성하기 때문에 아래와 같이 다른 레퍼런스를 가지게 된다.


```
String temp = "Temp";
String temp2 = new String("Baeldung");

System.out.println(temp.equals(temp2)); // true
System.out.println(temp == temp2); // false

```

때문에 같은 JVM위에서 올라가는 Kotlin도 마찬가지로 Java String Pool을 사용해서 아래와 같은 특성을 띄게 된다.

```
val temp = "Temp"
val temp2 = "Temp"

System.out.println(temp == temp2); // true
System.out.println(temp === temp2); // true

val temp = String(charArrayOf('T','e','m','p'))
val temp2 = String(charArrayOf('T','e','m','p'))

System.out.println(temp == temp2); // true
System.out.println(temp === temp2); // false


//String() 내부 코드
public actual inline fun String(chars: CharArray): String =
    java.lang.String(chars) as String
    
```

위와 같이 String()은 Java의 생성자를 가져와서 Kotlin의 String으로 캐스트하기 때문에, 다른 오브젝트가 생성되고, 때문에 코틀린의 레퍼런스 비교하는 '==='에서의 차이가 보이게 된다.
