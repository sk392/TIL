##rxThreadPool

Rx에서 병렬화 전략을 사용할 땐 주의해야 하는 점이 있는데, computation 스케쥴러를 제외한 다른 스케쥴러는 시스템에 너무 많은 스레드를 사용해서 성능의 저하를 이끌언 낼 수 있다. computation 스케쥴러는 CPU의 수에 따라 동시 프로세스 수를 제한하게 됩니다.

io() 쓰레드나 newThread()의 경우에는 동시 프로세스 수를 제한하지 않기 때문에 성능의 저하가 발생할 수 있고, 이 경우는 동시 프로세스 수를 제한하는 int 인수를 flatMap에 전달할 수 있다.

또한 필요한 경우 ExecutorService 를 사용하면 보다 세부적으로 이를 조절할 수 있고, 실제로 작업을 수행하면 상당히 향상된 성능을 얻을 수 있다. 이에 대한 자세한 것은 [병렬화 극대화](http://tomstechnicalblog.blogspot.com/2016/02/rxjava-maximizing-parallelization.html)에 대한 곳에서 찾아볼 수 있다. 

보다 자세한 사항은 [여기](http://tomstechnicalblog.blogspot.com/2015/11/rxjava-achieving-parallelization.html)에서 확인할 수 있다.