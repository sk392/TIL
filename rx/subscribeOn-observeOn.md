##SubscribeOn와 ObserveOn

SubscribeOn : 위치와 상관 없음. 여러 개 있으면 맨 위에꺼만 적용
ObserveOn : 위치 밑에부터 스케쥴러 적용

####observeOn

이 것을 호출한 이후의 다운스트림에만 영향을 미치게 된다!

![observeOn](https://cdn-images-1.medium.com/max/1600/0*8GhaR2CbbJW0Tmlz.gif)

가장 쉽게할 수 있는 오해 중 하나는 observeOn또한 업스트림에 영향을 미친다고 생각하지만, 실제로는 다운 스트림에서만 작동한다


#### subscribeOn
이것은 Observable를 Subscribe을 할 때 사용되는 스레드에만 영향을 미치며 다운 스트림에 남아있으며 위치는 중요하지 않다.


```
just("Some String") // Computation
  .map(str -> str.length()) // Computation
  .map(length -> 2 * length) // Computation
  .subscribeOn(Schedulers.computation()) // -- changing the thread
  .subscribe(number -> Log.d("", "Number " + number));// Computation
```

####다중 subscribeOn

subscribeOn스트림에 여러 인스턴스가 있는 경우 첫번 째만 유효하다.

```
just("Some String")
  .map(str -> str.length())
  .subscribeOn(Schedulers.computation()) // changing to computation
  .subscribeOn(Schedulers.io()) // won’t change the thread to IO
  .subscribe(number -> Log.d("", "Number " + number));
```
