## DI_Tutorial

이번에 DI를 사용해야해서 [튜토리얼](https://codelabs.developers.google.com/codelabs/android-dagger/#0)을 따라하면서 그냥 공부한 것들 들까먹으려고 공부하면서 메모를 해본다 쓱쓱..

### Annotataions

[Doc](https://docs.oracle.com/javaee/6/api/javax/inject/Inject.html) ```@Inject``` : constructor에 넣어주면 Dagger에서 해당 클래스의 인스턴스를 만들 수 있게 된다. 이 때 constructor에 들어 있는 파라미터도 Dagger에서 주입할 수 있어야 한다.

[Doc](https://dagger.dev/api/latest/dagger/Component.html) ```@Component``` :  Dagger가 의존성 그래프를 생성 및 관리하기 위해서 필요하며,  Scope의 역할과 ``` fun inject(activity: MainActivity)```라는 함수가 있으면 해당 함수의 생성자 및 전역 변수에 주입해주는 역할을 담당한다. 이 인터페이스는 생성 되면서 앞에 Dagger라는 이름이붙은 클래스를 생성한다.

[Doc](https://dagger.dev/api/latest/dagger/Module.html) ```@Module``` : Dagger에게 인스턴스를 제공할 수 있는 방법으로, ```@Provides, @Binds```를 사용해서 제공 해줄 수 있다. ```includes``` 와 ```subComponent```를 통해서  추가할 수 있는데, ```includes```는 다른 ```@Module```을 포함하고, ```subComponent ```는 ```@Subcomponent, @ProductionSubcomponent```어노테이션이 붙어 있는 컴포넌트를 하위 구성요소로 추가하는 방식이다

[Doc](https://dagger.dev/api/latest/dagger/Binds.html) ```@Binds``` : 인터페이스를 제공할 때 Dagger에게 사용해야할 인스턴스를 지정할 때 사용한다. 반드시 ```abstract``` 를 사용해야하고, 주어야할 인스턴스엔 ```@Inject```어노테이션이 필요하다.

[Doc](https://dagger.dev/api/latest/dagger/BindsInstance.html) ```@BindsInstance``` : 


### Questions
1. @Inject는 보통 기본생성자에 붙여주긴 하는데, 여러 Constructor가 있을 때 하나만 붙여주면 다른 형태는 주입이 안되는걸까..?
2. includes과 subComponent차이 테스트하기

### Memo

1. Activity, Fragment 등의 특정 Android Framework class들은 시스템에서 인스턴스화 하므로 Dagger에서 생성할 수 없다.