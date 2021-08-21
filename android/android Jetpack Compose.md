## Android Jetpack Compose

[Android JetPack Compose](https://developer.android.com/jetpack/compose)는 네이티브 UI를 위한 선언형 UI를 지원하는 라이브러리입니다.

### 주요 특징
* 코드 감소 : Android View에 비해 적은 코드로 더 많은 작업이 가능하며 kotlin, xml을 사용하지 않고 kotlin으로 사용가능.
* 직관적 : 선언적 UI를 활용하며 테마 레이어를 적용하기 용이, Activity, Fragment에 종속되지 않은 작은 stateless 구성요소를 테스트 가능(Preview Annotation) 사용하면됨
* 빠른 개발 : 기존 모든 코드와 호환 가능하며, viewmodel. coroutine등은 Comosable과 함께 호환됨. theme 관리를 안해도됨.
* 강력한 성능 : 머티리얼 디자인, 다크테마, 애니메이션 등을 기본적으로 지우너하며 접근성과 레이아웃을 모두 개선,
* 주요 사용 서비스의 사용기 : [트위터 Twitter](https://developer.android.com/stories/apps/twitter-compose), [스퀘어 Square](https://developer.android.com/stories/apps/square-compose), [쿠바 Cuvva](https://developer.android.com/stories/apps/cuvva-compose), [몬조 Monzo](https://developer.android.com/stories/apps/monzo-compose)


#### 간단 사용법
![예시](https://developer.android.com/images/jetpack/compose/mmodel-simple.png)


* 함수는 `@composable` 주석을 통해 UI를 그릴 수 있다.
* 함수는 파라미터로 name을 받고 해당 UI를 그려주기 때문에 아무것도 반환할 필요가 없다. (순수 함수로 나누어 문제를 해결하는 함수형 프로그래밍과 비슷하게, 같은 값을 받으면 항상 같은 UI를 그린다.) = 멱등적(idempotent) 특징을 지녔다.
* compose의 위젯은 stateless 상태이며 setter, getter를 노출하지 않는다.


----


### 재구성

![compose 데이터 전달 구조](https://developer.android.com/images/jetpack/compose/mmodel-flow-data.png)

![compose 이벤트 전달 구조](https://developer.android.com/images/jetpack/compose/mmodel-flow-events.png)

 사용자가 UI의 `onclick` 과 같은 이벤트를 발생시키면 이 이벤트를 통해 앱 로직에 전달하여 앱의 상태를 변경한다. 상태가 변경되면 composable 함수는 새로운 데이터와 함께 UI를 새로 구성한다. 이 과정을 재구성(recomposition) 이라고 한다.
 
  재구성이 일어날 때 전체 UI트리를 재구성하는데, compose는 이를 지능적 재구성(intelligent recomposition)을 통해 문제를 해결한다.
 재구성은 입력이 변경될 때 composable함수를 다시 호출하는데, 이 때 변경되었을 수 있는 함수 혹은 람다만 호춣하고 나머지는 건너 뛰어 매개변수가 변경되지 않은 경우는 UI를 재구성하지 않으니 compose의 재구성이 효율적으로 이루어진다. (kotlin의 equals나 hash 등을 더 조심해야할듯, 또 차일드뷰가 다시 그려지면 부모뷰도 영향을 받을 텐데 정말 효율적인 구성일까?)
  composable 함수는 MainThread에서 동작하므로 이 때 무거운 작업을 해서는 안되고 화면 렌더링이 느려져 유저에게 애니메이션 등이 버벅거리는 경험을 줄 수 있으므로 지양해야한다. 예시로는 다음과 같은 동작이 있다.

* 전달받은 object에 직접 쓰기
* viewmodel의 observable update
* shared preferences 수정하기

위와 같은 동작을 하기 위해선 고차함수를 통해 function을 전달받아 실행 하도록 진행해야한다. composable 함수 내에서는 하지말고! composable은 다음과 같은 특징을 지닌다.
 
* composable 함수는 순서와 관계없이 실행할 수 있다.
* composable 함수는 동시에 실행할 수 있다.
* 재구성은 최대한 많은 수의 composable함수 및 람다를 건너뛰어야 한다.
* 재구성은 낙관적이며 취소될 수 있다.
* composable 함수는 애니메이션의 모든 프레임에서와 같은 빈도로 매우 자주 실행될 수 있다.( onDraw와 같은 수준인 것같은데? 로그 찍는건 지양해야할듯)

 
https://developer.android.com/jetpack/compose/mental-model?authuser=2#any-order
 

-----


### CodeLab

* [1차 튜토리얼 코드랩](https://developer.android.com/codelabs/jetpack-compose-basics?hl=ko#0)   -완료
 
 1. 실제로 다양한 업무에 사용할 수 있을 것같다고 체감됨.
 2. 리스트는 현재 RecyclerVIew보다 성능이 좋진 않음, 다만 직관적
 3. Theme이나 텍스트 스타일 등을 맞추기 아주 좋음. (개수 제한은 있음 h1, h2, .. h6까지 밖에 지원안됨)

 