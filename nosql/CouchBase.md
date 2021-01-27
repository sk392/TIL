## CouchBase

NoSQL인 카우치베이스는 CouchDB(카우치디비)의 일종으로, membase 솔루션에 Apache의 CouchDB를 기반으로 새롭게 만든 솔루션. MongoDB나 Riak과 같이 Json document를 직접 저장할 수 있는 DocumentDB(?) 형태를 가지며, NoSQL의 분산 이론인 CAP theorem(?)에서 CP(Consistency & partition tolerance)의 부분에 해당 하며 데이터에 대한 일관성과 노드간의 네트워크 장애시에도 서비스를 제공할 수 있다. 

### 특장점

*  Membase를 기반으로 하였기 때문에 Memcached를 자체적으로 Level 2 캐쉬로 사용하고 있어 빠름
* CouchBase같이 CouchDB들은  iOS, OS X, tvOS, Android, Linux, Windows, Xamarin 등을 포함한 모든 주요 플랫폼을 지원
* NoSQL에서 Indexing, Grouping, Ordering. Join 가능(로직?)
* Memcached 프로토콜을 지원
* Couchbase Lite와 CouchBase Server간 동기화 및 Couchbase Lite인스턴스 간의 P2P 동기화 가능
* RestAPI 지원





#### 출처

* [카우치베이스 서버 소개 및 설치하기 -조대협의 블로그](https://bcho.tistory.com/924)
* [카우치베이스 모바일](https://developer.couchbase.com/mobile/)
