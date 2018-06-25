## 다양한 스케줄러

[Schedulers](http://reactivex.io/documentation/scheduler.html) 클래스의 팩토리 메서드를 사용해서 스케쥴러를 생성한다. 아래의 테이블은 RxJava에서 제공하는 메서드와 사용 가능한 스케줄러들을 보여준다

![scheduler](http://reactivex.io/documentation/operators/images/schedulers.png)

이 그림에서 알 수 있듯이 SubscribeOn 연산자는 연산자가 호출되는 연산자 체인의 어느 시점에서든지 Observable이 시작할 스레드를 지정한다. ObserveOn 은 Observable이 해당 연산자가 나타나는 위치 아래 에서 사용할 스레드에 영향을줍니다 . 이러한 이유 때문에 Observable 연산자 체인 중 다양한 지점에서 ObserveOn을 여러 번 호출 하여 특정 연산자가 작동하는 스레드를 변경할 수 있습니다.


| 스케줄러 | 용도 |
|:--------|:--------|
| Schedulers.computation( ) | 이벤트-루프와 콜백 처리 같은 연산 중심적인 작업을 위해 사용된다; 그렇기 때문에 I/O를 위한 용도로는 사용하지 말아야 한다(대신 Schedulers.io( )를 사용); 기본적으로 스레드의 수는 프로세서의 수와 같다  |
| Schedulers.from(executor) | 명시한 Executor를 스케줄러로 사용한다  |
| Schedulers.immediate( ) | 현재 스레드에서 즉시 실행할 작업을 스케줄링 한다   |
| Schedulers.io( ) | 블러킹 I/O의 비동기 연산 같은 I/O 바운드 작업을 처리한다. 이 스케줄러는 필요한 만큼 증가하는 스레드-풀을 통해 실행된다; 일반적인 연산이 필요한 작업은 Schedulers.computation( )를 사용하면 된다; 기본적으로 Schedulers.io( )이며 CachedThreadScheduler로,CachedThreadScheduler는 스레드 캐싱을 사용하는 새로운 스레드 스케줄러라고 생각하면 된다  |
| Schedulers.newThread( ) | 각각의 단위 작업을 위한 새로운 스레드를 생성한다   |
| Schedulers.trampoline( ) | 대기 중인 큐를 처리한 후에 현재 스레드에서 실행 될 작업 큐를 만든다  |

 
 
##RxJava Observable 연산자를 위한 기본 스케줄러


(..) :모든 오버로딩을 포함한다는 의미. 

| 연산자 | 기본 스케줄러 |
|:--------|:--------|
|buffer(..)|computation|
|delay(delay, unit)|computation|
|delaySubscription(delay, unit)|computation|
|interval|computation|
|repeat|trampoline|
|replay(..)|computation|
|retry|trampoline|
|sample(period, unit)|computation|
|skip(time, unit)|computation|
|skipLast(time, unit)|computation|
|take(time, unit)|computation|
|takeLast(..)|computation|
|takeLastBuffer(..)|computation|
|throttleFirst|computation|
|throttleLast|computation|
|throttleWithTimeout|computation|
|timeInterval|immediate|
|timeout(timeoutSelector)| immediate |
|timeout(firstTimeoutSelector, timeoutSelector)| immediate |
|timeout(timeoutSelector, other)| immediate |
|timeout(timeout, timeUnit)|computation|
|timeout(firstTimeoutSelector, timeoutSelector, other)| immediate |
|timeout(timeout, timeUnit, other)|computation|
|window(..)| computation |
|timestamp|immediate|
|timer| computation |