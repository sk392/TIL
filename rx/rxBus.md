##EventBus
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




##RxBus

[RxBus](https://github.com/Anadea/RxBus) 는 라이브러리로도 있지만, 직접 짜는 것도 그리 어렵지 않다. 이벤트 버스의 성질을 잘 생각해보면 주의 해야하는 점은, Hot Observable을 활용해서 등록 및 해지를 활용하면 된다.
 각 원하는 이벤트마다 하나의 객체를 만들어서 사용하면 된다. 

####다음은 예제코드이다 
~~이상한 점이 있다면 수정요청을 해라~~


```
public class RxEventBus {
    private HashMap<Object, List<PublishSubject>> mapsByType = new HashMap<>();
    private static volatile RxEventBus instance;

    private RxEventBus() {
    }

    public static RxEventBus getInstance() {
        if (instance == null) {
            synchronized (RxEventBus.class) {
                if (instance == null) {
                    instance = new RxEventBus();
                }
            }
        }
        return instance;
    }

    public <T> Observable<T> register( Class<T> eventClazz) {
        String tag =eventClazz.getSimpleName() ;
        List<PublishSubject> subjects = mapsByType.get(tag);
        if (subjects == null) {
            subjects = new ArrayList<>();
            mapsByType.put(tag, subjects);
        }
        PublishSubject<T> subject = PublishSubject.create();
        subjects.add(subject);
        return subject;
    }

    public void unregister(Object tag, Observable observable) {
        List<PublishSubject> subjects = mapsByType.get(tag);
        if (subjects != null) {
            subjects.remove(observable);
            if (subjects.isEmpty()) {
                mapsByType.remove(tag);
            }
        }
    }

    public void post(Object o) {
        String tag = o.getClass().getSimpleName();
        List<PublishSubject> subjects = mapsByType.get(tag);
        if (subjects != null && !subjects.isEmpty()) {
            for (Subject s : subjects) {
                s.onNext(o);
            }
        }
    }
}

```
```
RxEventBus.getInstance().post(new UiEvent.OnUpdateBookmarkStatus());

```