## Android Jetpack Compose

[Android JetPack Compose](https://developer.android.com/jetpack/compose)는 네이티브 UI를 위한 선언형 UI를 지원하는 라이브러리입니다.

### 주요 특징
* 코드 감소 : Android View에 비해 적은 코드로 더 많은 작업이 가능하며 kotlin, xml을 사용하지 않고 kotlin으로 사용가능.
* 직관적 : 선언적 UI를 활용하며 테마 레이어를 적용하기 용이, Activity, Fragment에 종속되지 않은 작은 stateless 구성요소를 테스트 가능(Preview Annotation) 사용하면됨
* 빠른 개발 : 기존 모든 코드와 호환 가능하며, viewmodel. coroutine등은 Comosable과 함께 호환됨. theme 관리를 안해도됨.
* 강력한 성능 : 머티리얼 디자인, 다크테마, 애니메이션 등을 기본적으로 지우너하며 접근성과 레이아웃을 모두 개선,
* 주요 사용 서비스의 사용기 : [트위터 Twitter](https://developer.android.com/stories/apps/twitter-compose), [스퀘어 Square](https://developer.android.com/stories/apps/square-compose), [쿠바 Cuvva](https://developer.android.com/stories/apps/cuvva-compose), [몬조 Monzo](https://developer.android.com/stories/apps/monzo-compose)

### CodeLab

* [1차 튜토리얼 코드랩](https://developer.android.com/codelabs/jetpack-compose-basics?hl=ko#0)   -완료
 
 1. 실제로 다양한 업무에 사용할 수 있을 것같다고 체감됨.
 2. 리스트는 현재 RecyclerVIew보다 성능이 좋진 않음, 다만 직관적
 3. Theme이나 텍스트 스타일 등을 맞추기 아주 좋음. (개수 제한은 있음 h1, h2, .. h6까지 밖에 지원안됨)

 