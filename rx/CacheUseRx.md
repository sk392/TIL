##Rx와 캐시

####캐시를 사용하는 방법 
1. [concat](http://reactivex.io/documentation/operators/concat.html)
2. [concatEager](http://reactivex.io/RxJava/javadoc/rx/Observable.html#concatEager(java.lang.Iterable))
3. [merge](http://reactivex.io/documentation/operators/merge.html)
4. [publish☆](http://reactivex.io/RxJava/javadoc/rx/Observable.html#publish(rx.functions.Func1)) + selector + merge + takeUntil


####concat
첫번째 observable이 끝난 후에 다음 Observable이 시작된다.
단, 첫번째 Observable이 끝나지 않으면 다음이 시작되지 않는다.


```
Observable.concat(            
		   	Observable.interval(1, TimeUnit.SECONDS).map(id -> "A" + id),
         	Observable.interval(1, TimeUnit.SECONDS).map(id -> "B" + id))
           .subscribe(System.out::println);
```
###### 결과 :A0 A1 A2 A3 A4 A5 A6 A7 A8 ...


####concatEager
concat을 보완하여 첫번째 Observable과 함께 다른 Observable도 함께 시작되지만, 반환한 결과는 첫번째 Observable이 끝난 후에 다음 Observable이 출력된다. 단, 두번째가 먼저 끝나더라도 버퍼로 기다림.

```
        List<Observable<String>> observables = new ArrayList<>();
        observables.add(Observable.interval(1, TimeUnit.SECONDS).map(id -> "A" + id));
        observables.add(Observable.interval(1, TimeUnit.SECONDS).map(id -> "B" + id));

        Observable.concatEager(observables)
                .subscribe();

```
###### 결과 :A0 A1 A2 A3 A4 A5 A6 A7 A8 ...


####merge

Observable이 먼저 끝난 순서대로 방출하게 된다. 더 빠르게 결과가 나온 쪽으로 바로 반영할 수 있음.
단, 새로운 데이터가 생겼을 때, 기존에 캐싱되어 있는 값(디스크)이 새로운 데이터(네트워크) 보다 느릴 경우 새 값을 기존의 캐시 값으로 덮어쓸 수 있다.

```
Observable.merge(
            Observable.interval(1, TimeUnit.SECONDS).map(id -> "A" + id),
            Observable.interval(1, TimeUnit.SECONDS).map(id -> "B" + id))
    .subscribe(System.out::println);
```
###### 결과 :A0 B0 A1 B1 B2 A2 B3 A3 B4 A4


####publish
이 문제를 해결하기 위해 merge를 publish"selector"와 함께 사용할 수 있다. publish가 네트워크를 Subscribe하고 그 것이 방출되기 전까지 디스크 캐시를 방출을 선택할 수 있게 된다. 네트워크 관찰 가능 항목이 방출되기 시작하면 관찰 가능한 디스크의 모든 결과가 무시되게 된다. 덕분에 우리는 위와 같은 고민을 하지 않아도 해결할 수 있게 된다.