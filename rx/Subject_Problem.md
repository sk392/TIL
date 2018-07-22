## Subject's Problem


일반적인 관측 가능 설정
실용적인 관점에서 볼 때, Observable일련의 작업은 세 부분으로 나뉩니다. 

1. Observable배출원이 발생한 출처 

2. 배출물을 변형시키는 운영자 

3. 배출물 Subscriber을 소비하는 배출원 

물론 # 2를 더 복잡한 것으로 만들 수 있습니다. 다른 Observables은 같은 작업을 merge()하고 flatMap(). 그러나 모든 Observable것이이 구조를 따라야합니다. 가장 간단한 예는 고정 된 String값 집합을 내보내고 길이를 매핑 한 다음 인쇄하는 것입니다.

```java
//Source
Observable<String> values = Observable.just("Alpha", "Beta", "Gamma");

//Operators
Observable<Integer> lengths = values.map(String::length);

//Subscriber 
Subscription printSubscription = lengths.subscribe(System.out::println);
```

Source, Operators, Subscriber 의 세 가지 구성 요소는 위에 명시 되어 있지만 이를 하나의 문장으로 표현할 수 있다..

```java
//Source, Operators, Subscriber
Observable.just("Alpha", "Beta", "Gamma")
    .map(String::length)
    .subscribe(System.out::println);
    
```

이 모든 Observable작업은 유연한 동시성에 대한 좋은 것은 예약 된 스레드에 observeOn()와 subscribeOn()를 쉽게 작동을 시킬 수 있다.  읽기하지만 먼저 자신을 관찰 가능한 그냥 소스를 살펴 보자 자신의 성격을 분석한다.