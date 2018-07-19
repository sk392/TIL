## Kotlin_Kapt

코틀린의 Kapt는 (Kotlin Annotation Processing)의 약자로서
Java6부터 도입되었으며 Pluggable Annotation Processing API (JSR 269)를 Kotlin에서도 사용 가능하게 된 것이다.

한가지 예를 통해 설명해보록 하자면

코틀린에서 Annotation Processing를 지원하기 위해서는 kapt를 사용해야 하고, build.gradle에 플러인을 추가하면 된다.

#####apply plugin: 'kotlin-kapt'

kapt를 사용하면 의존성에 annotationProcessor로 설정된 부분은 모두 kapt로 변경해야 하고, 마찬가지로 testAnnotationProcessor는 kaptTest로 변경해야 한다.

compile 'org.projectlombok:lombok:1.16.14'
kapt 'org.projectlombok:lombok:1.16.14'
이렇게 바꾸면 Lombok이 동작하지 않습니다. Lombok과 kapt는 호환되지 않기 때문인데 이 문제를 해결하기 위해서는 두가지 방법이 있다.


코틀린을 사용하면 Lombok을 사용할 필요가 없으므로, Lombok을 모두 제거하고 코틀린으로 변경하거나 자바와 코틀린 코드를 별도의 모듈로 분리하여 관리할 수 있다.
