##Oper - onErrorResumeNext

onErrorResumeNext.. 이건 옛날에도 알았지만 안쓰고 정리도 안하니까 까먹었다가 케빈덕분에 (고마워요 케빈웨건!) 생각나서 생각난김에 정리하려고 한다.

onErrorResumeNext는 Rx에서 Observable이나 Completable,Single(노올랍구나) 등에서 Error가 호출될 때 대신 호출될만한 Observable 등을 호출해준다.

데이터 가져올 때 메모리에 있는 친구 가져오고 없으면 리모트에서 가져오는 등의 코드에서 쓰기 적합하당

####OnErrorResumeNext

예제는 아래 코드와 같은데 memory에서 data를 가져올 때 없으면 remote에서 가져오는 코드이다. 

```
 fun getData(success : (result : Result) -> Unit, error : (it: Throwable) -> Unit) : Disposable{
 return memoryDataSource.getData() //1
 		 .onErrorResumeNext (remoteDataSource.getData)//2
 		 .subscribe({
 		 	success(it) //3
 		 },{
 		 	error(it) //4
 		 }) 
 }
  
```

간단하게 알아보기 쉽게 번호로 정리하자면 이렇게 나올 수 있다.

```
1(success) -> 3 -> done
1(error) -> 2(success) -> 3 -> done
1(error) -> 2(error) -> 4 -> done
```


##### 유사제품

* onError : Exception을 받을 수 있다
* onErrorReturn : Exceptioin이 던져진 경우에 대신할 데이터를 흘릴 수 있다
* onErrorResumeNext : Exception이 던져진 경우에 대신할 스트림을 흘릴 수 있 있다
* retry : Exception이 던져진 경우 재시도할 수 있다
* retryWhen : 재시도 타이밍을 세밀하게 제어할 수 있다

참고해볼만한 [링크](http://pluu.github.io/blog/rxjava/2017/02/26/rx-error/)