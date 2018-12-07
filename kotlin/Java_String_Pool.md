## Java String Pool

 코틀린은 자바와 같이 JVM위에 올라가고, 자바에서 쓰는 특수 메모리 영역인 Java String Pool을 사용한다.


### String Interning

  Java String Pool(이하 Pool)은 문자열의 불변성 때문에 Pool에 리터럴 문자열 하나만 저장하여 메모리 양을 최적화 하기 위해 사용한다. 이 과정은 Intenring()이라고 말한다.
 
 스트링 변수를 만들고 해당 값을 할당하면 (e.g. 
 ```
 val a = "temp" 
 ```
) JVM이 Pool에서 동일 한 값의 String을 검색하고, 발견되면 추가 메모리 할당을 하지 않고 단순히 메모리 주소에 대한 참조를 반환한당.(이것 떄문에 레퍼런스가 같음) 찾지 못하면 Pool에 추가되고 참조가 반환된당
 
 해당 인터닝은 수동으로도 할 수 있는데, 
 
 ```
 val a = "ddd"
            a.intern()

 ```
 위와 같이 진행한다면 Pool에 수동으로 올릴 수 있다.
 
### GC(Garbage Collection)

JAVA7 미만에서는 JVM이 Permenant Generation Space에 Java String Pool을 배치했고, 이 공간은 GC대상이 아니므로 런타임중에 확장하거나 제거할 수 없다. 때문에 Java String Pool에 많은 문자열이 들어가게 되면, OutOfMemory가 발생할 수 있다. 단, Java7 이상에서는 Heap 에 들어가서 GC 대상이 된다.


좀 더 자세한 내용은 [요기](https://www.baeldung.com/java-string-pool)에서 확인해볼 수 있다. 잘 정리가 되어있다!