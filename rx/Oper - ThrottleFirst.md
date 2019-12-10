## Oper - ThrottleFirst

[RxBinding](https://github.com/sk392/TIL/blob/master/android/android_rxbinding.md)에 대한 정리를 하다가 생각난 김에 정리하는 Rx - ThrottleFirst (자매품 [debounce](https://github.com/sk392/TIL/blob/master/rx/Oper%20-%20debounce.md))

debounce와 비슷하게 Android에서 중복 클릭 과 같은 것들을 방지하기 위해서 쓸 때 좋은 operator인데, 클릭한 후 일정시간의 이벤트를 전달하지 않도록 되어 있다. 

클릭 후 일정시간 동안 작업을 펜딩 한 후 처리하는 debounce와 다르게 throttleFirst는 먼저 작업을 처리한 후에 일정 시간동안 작업을 블럭 시키는 기능이 있다.

송금 기능과 같은 것이 버튼 하나로 바로 전달된다면 ~~(그럴리 없지만)~~ 빠르게 연속으로 누른 경우 송금으 여러번 될 수 있는데, 이 때 사용하게 적당한 operator라고 할 수 있다. rxBinding과 함께 사용하면 코드가 더 간결해질 수 있다.

ThrottleFirst는 아래와 같이 동작한다

![](https://user-images.githubusercontent.com/18481078/58364856-7fef7e80-7ef5-11e9-8413-d7441f568d51.png)