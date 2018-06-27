## oper - Publish

[Publish](http://reactivex.io/documentation/operators/publish.html)는 오퍼레이터중 하나로 Observable을 연결 가능한 Observable로 바뀌주는 역할을 한다.

![publish](http://reactivex.io/documentation/operators/images/publishConnect.c.png)


###Example 1


```

getNetwork().publish(new Function<Observable<List<Repository>>, Observable<List<Repository>>>() {
                @Override
                public Observable<List<Repository>> apply(Observable<List<Repository>> listObservable) throws Exception {
                    return  Observable.merge(getCache().takeUntil(listObservable), listObservable);
                }
            })
                    .subscribeOn(Schedulers.io())
                    .observeOn(AndroidSchedulers.mainThread())
                    .subscribe(repositories -> {
                        for (Repository repository : repositories) {
                            Repository cacheRepository = repositoryHashMap.get(repository.getId());

                            if (cacheRepository == null ||
                                    Long.parseLong(cacheRepository.getUpdatedAt()) < Long.parseLong(repository.getUpdatedAt())) {
                                repositoryHashMap.put(repository.getId(), repository);
                            }
                        }
                        mTextAdapter.setTextItems(getMapToList(repositoryHashMap));
                    });
```

위의 코드는 publish를 통헤서 seletor를 구현하고 Observable.merge에서 캐시 값을 가져올 때 takeuntil을 사용하여 network에서 작업한 결과물이 내려온 경우 캐시에서 값을 불러오는 것을 그만두고, 바로 네트워크의 값으로 대체한다. 

이는 순차적으로 값을 가져오는 다른 로직의 캐시보다 효율적이고, publish를 통해서 observable객체를 가져와 원하는 대로 핸들링할 수 있도록 되어 있다.