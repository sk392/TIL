## ConstraintLayout

ConstraintLayout은작년 Google I/O에서 처음 소개됐습니다. Constraint Layout이라는 개념은 개발자가 인터페이스를 더욱 풍부한 표현 방식으로 개발할 수 있도록 제공하기 시작했습니다. 개발자가 뷰 사이의 관계를 표현할 수 있도록 하기 위한 목적입니다. 따라서 Constraint Layout은 Relative Layout 보다 풍부한 표현력을 가집니다.

Constraint Layout은 플랫폼에서 언번들 형태로 제공되며 다음 두 가지 사항을 허용합니다.

* API 9 이상부터 Constraint Layout을 사용할 수 있습니다.
* 빠른 반복이 가능합니다.

#### ConstraintLayout 구성요소
 
* Comments 

제약 조건(Constraint)은 UI를 설정할 때 결정되는 관계, 즉 위치와 크기(차원)입니다.

* 방정식(Equation)

제약 조건으로 관계를 생성하면 시스템이 이를 선형 방정식 시스템으로 변환합니다. 설정한 제약 조건에 따라 뷰 위치의 미설정 값이 해결됩니다.

* 해법(Solver)

워싱턴대에서 개발된 Cassowary Linear Arithmetic Constraint solving algorithm이라는 해법을 사용합니다. iOS의 Autolayout에서도 사용하는 해법이죠. 방정식을 입력하면 위치와 뷰 크기를 반환해 줍니다.

