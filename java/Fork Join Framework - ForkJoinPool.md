# Fork Join Framework - ForkJoinPool

ForkJoinFramework는 쓰레드 풀의 일종이다. 보통의 쓰레드 풀과 다른 점은 ForkJoinPool은 큰 작업을 작은 작업 단위로 쪼개고, 그 것을 다른 CPU에서 병렬로 처리한 후에 결과를 취하는 방식을 가지고 있다는 점이다. (비슷한 것으로 [분할정복알고리즘](https://ko.wikipedia.org/wiki/%EB%B6%84%ED%95%A0_%EC%A0%95%EB%B3%B5_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)이 있다.)

ForkJoinFramework에서는 여러 CPU를 최대한 활용하면서 동기화와 GC를 피할 수 있는 여러 기법이 사용되어 Java뿐만아니라 Scala에서도 널리 쓰이고 있는 병렬처리 기법이다. 

### ForkJoinFrameWork의 특징 요약

1. 큰 작업을 작은 작업으로 쪼갠다.
2. 부모 쓰레드로 부터 처리로직을 복사해 새로운 쓰레드에서 쪼개진 업무를 수행(Fork)시킨다.
3. 2번을 반복하다가 특정 쓰레드에서 더이성 Fork가 일어나지 않고 업무가 완료되면 그 결과를 부모 쓰레드에서 Join하여 값을 취한다.
4. 3을 반복하다가 최초의 ForkJoinPool을 생성한 쓰레드로 값을 리턴하여 작업을 완료한다.



<p float="left">
  <img src="http://postfiles15.naver.net/20160609_158/2feelus_1465431425719GQ0V2_PNG/2016-06-09_at_9.17.47_AM.png?type=w2" width="300" />  
  <img src="http://postfiles3.naver.net/20160609_162/2feelus_1465431425334z7mdz_PNG/2016-06-09_at_9.18.01_AM.png?type=w2" width="300" />
</p>


### ForkJoinPool의 성능의 핵심 : Work-Stealing


기본적으로는 newCachedThreadPool이나 newFixedThreadPool처럼 ExecutorService의 구현체이다. 그러나 일반 ExecutorService 구현 클래스와는 기본적으로 다른점이 하나 존재하는데 work-stealing algorithm이 구현되어있다는 점이다.


일반적으로 멀티쓰레드 프로그래밍을 할때 어려운 점중의 하나는 CPU자원을 골고루 사용하여 최대한의 성능을 뽑아내는 일이다. 정확히 어느정도의 CPU자원이 소모되는 일인지 가늠하기 힘든경우 Task를 어떻게 나누는지에 따라서 노는(Idle) CPU 시간이 많아질수도 있다.


ForkJoinPool의 work-stealling은 이런 경우를 효과적으로 해결한다.

1. 큰 작업이 들어온 경우에 Fork를 통해 업무 분할을 한다.
2. 분할 된 업무는 아래 그림처럼 submit과정을 통해 inbound queue에 쌓인다.
3. queue에서 들어온 작업들은 ForkJoinPool에 할 당된 CPU개수(여기서는 A,B 두개) 만큼의 쓰레드 개수로 작업이 분산된다.
4. 이 때 각 쓰레드는 내부적으로 Queue를 가지고있고(정확히는 Dequeue) 해당 큐에 작업을 추가한 후에 차례대로 실행한다.
5. 그런데 이때 B쓰레드 처럼 모든 일을 다 처리한 경우에 CPU자원이 놀게되는데(idle), 이런 상황에서 work-stealling이 동작하면서 A쓰레드 큐에서 남은 작업을 B로 가져와 처리함으로써 최적의 성능을 낸다. 
6. A가 B에게 어떤 작업을 훔쳐가면되는지 알려주는데, 이 때 마땅한게 없다면 Fail메시지를 날린다. 
7. B는 상황에 따라 A의 쓰레드에서 업무를 훔칠지 shared inbound queue에서 가져올지 결정한다.



![](http://postfiles2.naver.net/20160610_241/2feelus_1465489835811BcaiD_PNG/2016-06-10_at_1.30.19_AM.png?type=w2)







출처 : http://blog.naver.com/PostView.nhn?blogId=2feelus&logNo=220732310413
