## Oper - Debounce

[RxBinding](https://github.com/sk392/TIL/blob/master/android/android_rxbinding.md)에 대한 정리를 하다가 생각난 김에 정리하는 Rx - Debounce

특별히 정리할 것은 없지만. debounce는 Android에서는 중복 클릭 방지 등에서 사용 되는데, 특정 시간을 정해주면(예를 들면 1초) 해당 시간동안 클릭에 대한 결과를 전달하지 않고 대기한다.

조심해야할 포인트는 대기시간이 만약에 2초라 했을 때 클릭한 후 1초 후에 재클릭하면 재클릭 기준으로 2초를 다시 기다린다는 점이다. 그래서 만약 사용자가 1초 간격으로 계속해서 클릭하다보면 해당 기능은 영원히 실행 될 수 없다. 이렇게 대기 기간을 너무 길게 잡는 경우 사용자는 동작하지 않는 줄 알고 앱에 나쁜 리뷰를 달지도 모른다.

Debounce는 아래와 같이 동작한다

![](https://user-images.githubusercontent.com/18481078/58364779-35213700-7ef4-11e9-8eb6-eedc29e31cd8.png)