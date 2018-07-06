##rx-Paralllization

Rx에선 쉽게 오해하는 것이 그냥 subscribeOn을하면 알아서 병렬처리가 된다고 생각하는데 실제론 그렇지 않다.

```
Observable<Integer> vals = Observable.range(1,10);

vals.subscribeOn(Schedulers.computation())
          .map(i -> intenseCalculation(i))
          .subscribe(val -> System.out.println("Subscriber received "
                  + val + " on "
                  + Thread.currentThread().getName()));
```

```
// 결과
Calculating 1 on RxComputationThreadPool-1
Subscriber received 1 on RxComputationThreadPool-1
Calculating 2 on RxComputationThreadPool-1
Subscriber received 2 on RxComputationThreadPool-1
Calculating 3 on RxComputationThreadPool-1
Subscriber received 3 on RxComputationThreadPool-1
Calculating 4 on RxComputationThreadPool-1
....
```

그렇다면 하나 이상의 스레드에서 병렬처리를 하려면 어떻게 해할까?그리고 옵저버블의 계약에 어긋나지 않으려면?

방법은 바로 FlatMap에 있다.

```
Observable<Integer> vals = Observable.range(1,10);

vals.flatMap(val -> Observable.just(val)
            .subscribeOn(Schedulers.computation())
            .map(i -> intenseCalculation(i))
).subscribe(val -> System.out.println(val));
```

```
//결과
Calculating 1 on RxComputationThreadPool-3
Calculating 4 on RxComputationThreadPool-2
Calculating 3 on RxComputationThreadPool-1
Calculating 2 on RxComputationThreadPool-4
Subscriber received 3 on RxComputationThreadPool-1
Calculating 7 on RxComputationThreadPool-1
....
```

Observable의 계약은 중 하나는 Observable은 동시에 여러개의 onNext를 호출할 수 없다는 것인데, flatMap에서 필요한 모든 처리를 하게 되면 각각의 Observable에서 onNext를 호출하기 때문에 Observable의 계약을 어기지 않으면서 병렬처리를 쉽게 할 수 있다.

하지만 Rx에서 병렬화 전략을 사용할 땐 주의해야 하는 점이 있는데, computation 스케쥴러를 제외한 다른 스케쥴러는 시스템에 너무 많은 스레드를 사용해서 성능의 저하를 이끌언 낼 수 있다. computation 스케쥴러는 CPU의 수에 따라 동시 프로세스 수를 제한하게 됩니다.

io() 쓰레드나 newThread()의 경우에는 동시 프로세스 수를 제한하지 않기 때문에 성능의 저하가 발생할 수 있고, 이 경우는 동시 프로세스 수를 제한하는 int 인수를 flatMap에 전달할 수 있다.

또한 필요한 경우 ExecutorService 를 사용하면 보다 세부적으로 이를 조절할 수 있고, 실제로 작업을 수행하면 상당히 향상된 성능을 얻을 수 있다. 이에 대한 자세한 것은 [병렬화 극대화](http://tomstechnicalblog.blogspot.com/2016/02/rxjava-maximizing-parallelization.html)에 대한 곳에서 찾아볼 수 있다. 

보다 자세한 사항은 [여기](http://tomstechnicalblog.blogspot.com/2015/11/rxjava-achieving-parallelization.html)에서 확인할 수 있다.