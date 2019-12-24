
# Exploring Kotlin’s hidden costs - Part.2


Kotlin의 숨겨진 코스트들이 뭐가 있는지 정리해보자  2탄의 글은 [여기](https://medium.com/@BladeCoder/exploring-kotlins-hidden-costs-part-2-324a4a50b70) 있다.


## Local functions
 
 local funciton이란 다른 function 내부에 선언된 function을 말하며 function 내부에서만 접근할 수 있다.
 
 ```
fun someMath(a: Int): Int {
    fun sumSquare(b: Int) = (a + b) * (a + b)

    return sumSquare(1) + sumSquare(2)
}

 ```
 
 local function의 가장 큰 문제점은 local function은 inline으로 선언할 수 없고, local function을 포함한 function도 inline으로 선언할 수 없습니다. 이 문제는 해결할 수 없습니다.
 
 컴파일 후에 local function들은 ```Function``` Object로 변환되고, 람다와 마찬가지로 inline이 아닌 함수와 관련해서 [이전 설명](https://github.com/sk392/TIL/blob/master/kotlin/kotlin_hidden_costs.md)에서 설명한 것과 같은 제한이 생깁니다. Java로 Decompile하면 다음과 같습니다.
 
 ```
 public static final int someMath(final int a) {
   Function1 sumSquare$ = new Function1(1) {
      // $FF: synthetic method
      // $FF: bridge method
      public Object invoke(Object var1) {
         return Integer.valueOf(this.invoke(((Number)var1).intValue()));
      }

      public final int invoke(int b) {
         return (a + b) * (a + b);
      }
   };
   return sumSquare$.invoke(1) + sumSquare$.invoke(2);
}

 ```
 
 이런 방법은 실제 function의 인스턴스가 caller를 알고 있기 때문에 synthetic method 대신에 특정 method를 바로 호출할 수 있어서 Lambda에 비해서 method call이  1회 더 적습니다.
 
 즉, 외부에서 local function을 호출할 때 primitive Type의 casting이나 boxing이 발생하지 않는다는 것을 의미합니다. 바이트코드로 보면 다음과 같습니다.
 
 ```
 ALOAD 1
ICONST_1
INVOKEVIRTUAL be/myapplication/MyClassKt$someMath$1.invoke (I)I
ALOAD 1
ICONST_2
INVOKEVIRTUAL be/myapplication/MyClassKt$someMath$1.invoke (I)I
IADD
IRETURN
 ```
 
