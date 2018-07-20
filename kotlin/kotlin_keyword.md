## reified

```
fun <T> TreeNode.findParentOfType(clazz: Class<T>): T? {
  var p = parent
  while (p != null && !clazz.isInstance(p)) { 
    p = p?.parent
  } 
  
  @Suppress("UNCHECKED_CAST") 
  return p as T
}
```

위 함수는 노드가 특정 타입을 가졌는지 확인하기 위해 트리를 탐색하고 리플렉션을 사용하는 코드이다. 위 함수를 호출할 때 다음과 같이 호출할 것이다. findParentOfType(SomeClass::class.java) 이렇게 파라미터로 보기 좋지 않은 값을 전달하게 된다. 우리는 조금 더 간결하게 findParentOfType<SomeClass> 이렇게 표현하고 싶다. 그럴 경우 reified를 이용하여 인라인 함수를 정의하면 된다.

```
inline fun <reified T> TreeNode.findParentOfType(): T? { 
  var p = parent
  while (p != null && p !is T) { 
    p = p?.parent
  }
  
  return p as T 
}
```

위와 같이 인라인 함수를 정의하였다면 findParentOfType<SomeClass>() 이렇게 호출할 수 있다. 타입 파라미터에 reified를 정의하면 마치 클래스처럼 타입 파라미터에 접근할 수 있다. 인라인 함수이므로 리플렉션이 필요 없고 is나 as와 같은 일반 연산자가 동작한다. 참고로 인라인이 아닌 일반 함수에는 reified를 사용할 수 없다.