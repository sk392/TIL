## 2019_Droidknights Android


### Android TDD 적용기




##### TDD & AndroidTest 알아보기

테스트를 시작하는건 기획서나 usecase가 될 수 있음.

Red,Green, Refactor로 나뉘어서 3가지만 기억하면됨

테스트는 UnitsTest,IntegreationTests, e2eTests

UnitsTest는 클래스나 메서드들을 테스트하고, 이건 앱의 동작을 보장하진 않음

나머지 2개는 느리지만 앱의 동작을 보장함, 

이 3가지는 균형을 잘 이루어져야함.

70 /20 / 10의 비율로 하길 권장함

UT - 모키토나 에스프레소
나머지 - 안드로이드 서포트라이브러리 등

TDD를 먼저하려면 아키텍쳐를 먼저를 적용해야함

Architecture는 어떤걸 사용하나 상관없지만 Testable해야됨.
Testable은 응용프로그램의 부분을 분리와 개발을 유지관리할 수 있어야됨.

각 혁할의 인터페이스를 구현해서 테스트는 인터페이스기준으로 동작하는걻 ㅗ장ㄴ하는건가...

흠.. 외부에서 주입받아서 해야지 인터페이스를 뷰인터페이스랑 레포지터리 인터페이스

뷰에서 인터페이스 작업은 어떻게 성공을하는거지 흠.......


##### TDD만들어보기

```

@runWith(moktieo)
class loginPresenterTest{
	@mock
	private lateinit var view : LoginContract.View...
	
	inOrder
	순서정할 수 있음.
}

```

테스트 코드 작성원칭
given = 테스트를 위한 특정상황
when - 특정 액션이 바생했을떼ㅐ
then - 변화된 상태나 수행되는 행동을 검증한다.

```
@test
fun test_fail(){

	'when'(view.)...
	
}

```

위와같은 테스트 코드를 먼저 작성한 후에 interface에 해당 메소드를 추가한다(인터페이스에 먼저 메소드를 만들지않는다)

그렇게 텍스트를 실행하면 테스트가 실패한다(RED구간), 프레젠터에 아무런 코드를 작성하지 않았기 떄문,

그 다음에 테스트코드를 작성해서 성공시킨다.(Green구간)

그 다음은 리펙토링 (코드가 더러우니 깔끔하게 공통메시지를 빼던가 클래스로 뺀다. 그 후에 다시 테스트를 하고 통과하면 완료(Refactor)

이렇게 테스트 코드가 완료가 되면, 해당 테스트 코드를 목기능만 빼주면 실제 코드로 만들 수 있지. 개꿀띠?



### Android Mutli Module

Android Module이나 Libarary은 같은 구조를 가지고 있다

Android App Mudule, Android Library -> APK , Java Library -> JAR




### Android CleanAchitecture


엉클밥의 문서와 세션을 기반으로 함..

플랫폼에 특성에 대해 설명한다. -> platform -> IOchannel

플랫폼,Io채널보다는 수행하고자 하는 바가 이 app의 가장 중요한 버전이다.

acritecture가 플랫폼에 종속종인가? 의도에 대한것이지 프레임웤크에 대한 것은 아니다.

클린아키텍쳐가 가진 문제의식은 여기까지임.

##### useCase

핵심적인 기능을 정의하고 구현한 것.

entity = Business Object 근데 이건 DB나 이런것드를 반드시 참조해야되는데, 이걸 외부 세계외 나누기 위해서 interfface adatper를 두는걸로!

비즈니스로직을 바뀔 수 있는 모든 것으로 부터 분리하자 (플랫폼 종속 등등)

##### 의존성 규칙

안쪽원에서 바깥쪽 원을 참조할 수 없음. 

구현 방식이 2012년에 맞지 않고 2019년에 맞도록 구현체를 보고 개념에 조금 더 집중하는게 맞음.ㅣ

https://github.com/sunghyunzz/android-clean-architecture-example



개념적인 프로그램 =View
플랫폼과 독립적이고, 

#### 클린아키텍쳐 루머

CleanAcrhitectue는 mvp, mvvm과 비교하는 것은 맞지않다.
Clean이 꺠끗한게 아니라 엉클밥의 패턴이라고 볼 수 있음ㅋㅋㅋ
CleanArchitecture가 modern 하지 않은 정신은 아니다.. 모던한지 아닌지 상관없음.
CleanArchitecture는 DDD와 비슷하다. 입장은 비슷하지만, DDD의 다양한 개념들을 사용하진 않음



#### 지난 오해들

InterfaceAdapters가 반드시 필요함

Entity 

Domain은 클린아키텍쳐
Repository는 사용하지 않음, Entity GateWay패턴을 쓰는게 낫다. 
Presentation 로직






### 라지스케일 앱에서의 모든 아키텍쳐 

아키텍쳐에서 2가지가 중요함 테스트 가능성과, 재사용이 가능한가

테스트 =Tightly-coupled 때문에 테스트가 불가능함. 다른

재사용성 = 

fat Activity,fragment - 만악의 근원

Activity가 할 수 있는 일이 너무 많음.

nonmvc의 설계 전략

activity는 컨트롤러

mvp의 궁극적인 접근법= activity에서 view와 controller의 역할을 최대한 빼앗아 Flow만

mvvm - activitt만 컨텍스트만 의존, viewlogic은 databinding

AAC만 지원하는가 = Activity,Fragment의 형태를 꺠뜨리지 않으면서 재사용 생산성 이 높은 아키텍ㅊ펴 구현이 가능하기 떄문(추측잼)

##### mvp

mvp씀, netflix?

##### redux

출발점 : 모바일에서 가ㅏㅇ 큰 난제는 복잡한 상태들을 어떻게 깔끔하고 유지보수성 높게 구현할 것인가의 문제,

앱 전체의 이벤트를 state machine으로 처리한다면?

상태, 상태천이, 로직 수행

flux구조!

mvi는 안드로이드 개발에선 flux와 같은말

redux fux패턴에서 변형 구현체중 가장 효율적이라고 알려짐.

장점 : 상태처리에서 라이프사이클을 좀 척리가능,

Flux/redux 현재와 미래


rekotlin, rxRedux가 있으나 직접 구현이 좋음

Rx보단 coroutine이 훨씬 깔끔해서 이렇게 넘어감..



#### 2019 MutliModule!

internal class( 다른 모듈에서 보지 않게 하는 것)

compile -> implementation
멀티모듈지우너히강화되면서 추가됨 개념

컴파일은 손자모듈이 바뀔 때 할아버진 안해도됨
impli는 할아버짂지 안해도됨


모듈을 어떻게 나누나, Presentation.Domain,dataLayer



2018 google io cleanArchite ture domainLayer의 usecase 

mvvm 구조 

2917 droidkaigi 2019



### Window 마스터
layout 디스펙터


##### kitkat 

반투명 스테이터스바

Lollipop - 시스템바 백그라운드가 우리 윈도우 안에 위치하는 것을 말함

반투명 시스템바가 모든 커스텀 컬러보다 우선시함.


##### Window fit 방식

fitsystemWindows="true" ?? 시스템 ui로 확장해줌.

view의 status바나 ui바가 사이즈 안에 넣어서 해당 레이아웃과 겹치지 않게만듬. false면 statusqk

fullScreen은시스템 유아이 안쪽으로 padding을 넣어줌, 전체화면할건데, status바만큼내려주고싶으면 고

drawerlayout, coordinatorLayout등등에느 다른 동작을함

WTF를 알아서 설정해줌근데 이거해줄려면 자식뷰들도 추가해줘야댐.

일반적으론 view에 padding을 추가해서 시스템 ui와 겹치지 않게 동작하게 끔난들어줌..

WTF를 자동으로 설정해서 시스템 UI뒷편으로 드로우가 가능하게 만들 수 있다.

WTF는 풀스크린!


Window fit 방식 - 매뉴얼한 방식

하지말아야할 짓

status바의 높이를 임의로 지정하지 않기.보통은 24dp라고 알려졌는데 이걸 고정ㅇ로 paidding을 넣지 않기..

statusbar에 지정된 높이를 지정해서 가져올 수 있는데, 리소스 네임을 변경하게 된다면 


해야할 방법

windowsinsets를 사용하면됨.

시스템윈도우가 어디에 위치할지, 보여지는지 등을 제공해줌.

view에 listsner를 달면됨, ViewCompat이 있음. 

windowsInsets은 consume될때까지 전달한다. 각 차일드뷰에 순서대로 넘어간다. 


### display cutout (노치)

여기서 인셋을 활용해야됨

 never -> 오버래핑 못함
 default -> 확장 허용, systembar영ㅕㄱ에 완전히 포함이 되는 경우만
 landscape
 
 sshortedges - 가로모드던 아니던 확장가능, 
 
 getboundingRects는 노치가운데 부분의 영역을 찾아보는건.


### Dagger vs Koin

DI : 여러 클래스에서 공동으로 만들어서 사용하고 있는 모듈을 사용하면, DI를 통해 한 곳에서 만들고.. 유지보수가 편함
IOC : 그렇게되면 하나의 클래스에서 모든걸 관리하는게 아니라 DI를 통해 DI가 컨트롤하는거임.

Dagger1 : 리플렉션이 느리고, 런타임에 실행하기 때문에 런타임 크래시가 많이남. graph구성
Dagger 2(google) : annotation, 리플렉션안함, proguard안해도됨, 읽기 쉬움, 순수 자바,

##### Dagger2

필요한 개념 : injection, component, subcomponent, module, scope, provides

@module : 공급자
@componnent : 연결해주는 제공자
@inject : 주입받는 소비자


##### koin

코인은 순수 코틀린으로 만들어져있고, 

service locator pattern : 필요에 따라 서비스 오브젝트를 반환

DSL : 

koin : module, factory, single(bean) , bind, get, scope

refied function = 클래스처럼 타입파라미터로 접근할 수 있도록.

inline함수에서만 사용가능




















