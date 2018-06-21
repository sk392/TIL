## RxJava의 멀티 캐스팅

멀티 캐스팅은 RxJava에서 중복 된 작업을 줄이기위한 핵심 방법이다.
이벤트를 멀티 캐스트하면 모든 다운 스트림 운영자 / 가입자에게 동일한 이벤트를 보내고 이를 통해 네트워크 요청과 같이 많은 리소스를 사용하는 작업을 중복하여 실행하지 않을 수 있다.

멀티캐스트는 2가지 방법으로 할 수 있는데, 

하나는 ConnectableObservable( publish or reply)를 사용하는 것이고 나머지 하나는 subject를 사용하는 것이다.

여기서 주의해야하는 점은 멀티캐스트를 적절한 시점에서 하는 것인데 다음을 보면

####잘못된 코드

```
Observable<String> observable = Observable.just("Event")  
    .publish()
    .autoConnect(2)
    .map(s -> {
      System.out.println("Expensive operation for " + s);
      return s;
    });

observable.subscribe(s -> System.out.println("Sub1 got: " + s));  
observable.subscribe(s -> System.out.println("Sub2 got: " + s));

// Output:
// Expensive operation for Event
// Sub1 got: Event
// Expensive operation for Event
// Sub2 got: Event

```

위의 코드는 커다란 리소스를 잡아 먹는 map을 2번이나 사용하게 된다 이를 그림으로 나타내면 다음과 같다

![warning](http://i.imgur.com/txYelPq.png)

이를 수정하면 아래와 같아 지는데 멀티캐스트를 하는 시점에 따라 리소스 사용이 다르니 주의해야 한다.

####적절한 코드

```
Observable<String> observable = Observable.just("Event")  
    .map(s -> {
      System.out.println("Expensive operation for " + s);
      return s;
    })
    .publish()
    .autoConnect(2);

observable.subscribe(s -> System.out.println("Sub1 got: " + s));  
observable.subscribe(s -> System.out.println("Sub2 got: " + s));

// Output:
// Expensive operation for Event
// Sub1 received: Event
// Sub2 received: Event
```

![good](http://i.imgur.com/Yf1C8vu.png)


좀 더 자세하게 보고 싶다면 [여기](http://blog.danlew.net/2016/06/13/multicasting-in-rxjava/)를 눌러 확인해보자!

