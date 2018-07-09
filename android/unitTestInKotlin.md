## Kotlin의 단위테스트 (with Mockito)

#### Gradle 구성

의존성에 jUnit을 추가해야합니다. 프로젝트를 만들 때 이미 기본적으로 포함되어있을 가능성이 있습니다. 나중에 사용할 것이므로 Mockito도 추가 할 것입니다.

```

testCompile "junit:junit:4.12"
testCompile "org.mockito:mockito-core:1.10.19"
1
2
testCompile "junit:junit:4.12"
testCompile "org.mockito:mockito-core:1.10.19"
```

#### 첫 번째 테스트 만들기

에서 app/src/test폴더 (아직 존재하지 않는 경우 당신이 그것을 만들 수 있습니다), 당신은라는 새로운 클래스를 만들 수 있습니다 MyTest다음과 같습니다 :

```
class MyTest {

    @Test
    fun testsWork() {
        assertTrue(true)
    }
}
```

```
class MyTest {
 
    @Test
    fun testsWork() {
        assertTrue(true)
    }
}
```

보시다시피 Java에서 익숙한 것과 매우 유사합니다.


#### Mockito 사용 방법
Kotlin의 Mockito는 다른 라이브러리와 마찬가지로 사용할 수 있지만 해결해야 할 몇 가지 문제를 발견 할 수 있습니다.

```
@Test 
fun emptyDatabaseReturnsServerValue() {
    val db = Mockito.mock(ForecastDataSource::class.java)
    val server = Mockito.mock(ForecastDataSource::class.java)
    `when`(server.requestForecastByZipCode(any(Long::class.java), any(Long::class.java)))
            .then { ForecastList(0, "city", "country", listOf()) }

    val provider = ForecastProvider(listOf(db, server))
    assertNotNull(provider.requestByZipCode(0, 0))
}
```

모든 것이 매우 비슷합니다. 모의 객체를 만들어 코드 전체에서 원활하게 사용할 수 있습니다. 여기서는하지 않지만 'MockitoJUnitRunner'및 주석을 사용할 수도 있습니다.

이 단어 when는 Kotlin에서 예약어이므로 쉼표를 사용하거나 심지어 이름을 바꿀 수 있으며 import원하는 이름을 지정할 수도 있습니다.

```
import org.mockito.Mockito.`when` as _when
```

문제는 null 값을 허용하지 않는 유형을 모의하려고 할 때 발생합니다. 기본적으로 Mockito는 조롱 된 객체에 null 값을 제공하므로 조만간 문제가 발생합니다 .

약간의 트릭은 mockito-kotlin 과 같은 라이브러리를 사용하는 것입니다.이 라이브러리는 null을 사용하는 대신 기본적으로 각 유형에 특정 값을 제공하여 해당 문제를 해결합니다. 또한 Kotlin의 기능을 이용하여 작업을 단순화하는 다른 기능을 제공합니다 . https://antonioleiva.com/mockito-2-kotlin/

또 다른 문제는 기본적으로 Kotlin의 모든 클래스와 함수가 닫히기 때문에 확장 할 수 없다는 것입니다. Mockito는 이러한 문제를 조롱 할 수 없습니다.