##RxBus
안드로이드에는 [EventBus](https://github.com/greenrobot/EventBus)라는 라이브러리가 있다.
안드로이드는 Activity와 Fragment와 같이 화면에 종속된 부분이 많고, 때문에 각 Lifecycle이라던지, 고려해줘야 하는 상황들이 있다.
이 때 쓰는 것이 바로 EventBus인데 EventBus는 아래와 같은 구조로 되어있다.

![이벤트버스](https://github.com/greenrobot/EventBus/blob/master/EventBus-Publish-Subscribe.png?raw=true)

이벤트 버스의 특징은 다음과 같다.

1. simplifies the communication between components

	1. decouples event senders and receivers

	2. performs well with Activities, Fragments, and background threads
	3. avoids complex and error-prone dependencies and life cycle issues

2. makes your code simpler

3. is fast

4. is tiny (~50k jar)

5. is proven in practice by apps with 100,000,000+ installs

6. has advanced features like delivery threads, subscriber priorities, etc.

이러한 이벤트버스를 Rx로 구성한게 Rx버스이다.
