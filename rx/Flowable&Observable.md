# Flowable 과 Observable 의 차이

Rx가 버전 1->2로 업그레이드 되면서 추가 된 것중에 하나가 Flowable인데,
Flowable은 Observable과의 차이는 backpressure buffer의 기본으로 달려 있다.

## backpressure??


생산자는 미친듯이 element 를 생산해 내는데 소비자가 처리하는 속도가 이를 따라가지 못한다면 busy waiting 또는  out of memory exception 이 발생할 것이다. publish / subscribe 모델에서도 발생 가능
이에 대한 버퍼가 바로 backpressure buffer 다. 버퍼가 가득 차면 어차피 소비자는 element 를 처리할 여유가 없는 상태이므로 더 이상 publish 를 하지 않는다.

생산자의 생산 속도를 소비자가 따라가지 못하는 시나리오다.
Flowable 을 사용하면 default buffer size(128) 이상 backpressure buffer 에 element 가 쌓일 경우 흐름제어를 한다.

기존에 없던 개념이 새로 추가된 것은 아니다. 기존 rxJava 1.xx 의 경우 Observable 에 backpressure buffer 를 직접 생성해 주면 사용이 가능하다.

```
//흐름제어를 통해 정상 동작하는 코드

public class example01 {
 
    public static void main(String... args) throws InterruptedException {
 
        final String tmpStr = Arrays.stream(new String[10_000_000]).map(x->"*").collect(Collectors.joining());
        Flowable foo = Flowable.range(0, 1000_000_000)
                .map(x-> {
                    System.out.println("[very fast sender] i'm fast. very fast.");
                    System.out.println(String.format("sending id: %s %d%50.50s", Thread.currentThread().getName(), x, tmpStr));
                    return x+tmpStr;
                });
 
        foo.observeOn(Schedulers.computation()).subscribe(x->{
            Thread.sleep(1000);
            System.out.println("[very busy receiver] i'm busy. very busy.");
            System.out.println(String.format("receiving id: %s %50.50s", Thread.currentThread().getName(), x));
        });
 
        while (true) {
            Thread.sleep(1000);
        }
    }
}
```

```
//Observable을 backpressure buffer 생성 없이 사용하면 OutOfMemoryException

public class example02 {
 
    public static void main(String... args) throws InterruptedException {
 
        final String tmpStr = Arrays.stream(new String[10_000_000]).map(x->"*").collect(Collectors.joining());
        Observable foo = Observable.range(0, 1000_000_000)
                .map(x-> {
                    System.out.println("[very fast sender] i'm fast. very fast.");
                    System.out.println(String.format("sending id: %s %d%50.50s", Thread.currentThread().getName(), x, tmpStr));
                    return x+tmpStr;
                });
 
        foo.observeOn(Schedulers.computation()).subscribe(x->{
            Thread.sleep(1000);
            System.out.println("[very busy receiver] i'm busy. very busy.");
            System.out.println(String.format("receiving id: %s %50.50s", Thread.currentThread().getName(), x));
        });
 
        while (true) {
            Thread.sleep(1000);
        }
    }
}
```