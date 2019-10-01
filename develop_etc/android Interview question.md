## 안드로이드 면접 질문

adb를 사용하다가 zsh로 터미널을 대체한 후에 adb가 안되서 확인해보닝 bash_profile에 해당 path가 없어서 그렇다고 한당.


```

Binder 개념과 Binder에서 발생 가능한 문제

Zygote

프로세스 우선순위

LMK와 OOM Killer의 차이

프로세스 분리 이유 동작

Http Call 방법(library?)

ActivityThread

ActivityThread와 ActivityManagerService간의 통신

ANR

Looper/Handler/MessageQueue

 - Looper는 몇 개?

Message object pool, Message.obtain 쓰는 이유.

 - pool의 개수

 - pool은 어디에 어떤 형태로?

Handler 용도

ViewParent, getParent

ViewRootImpl의 용도는?

AsyncTask 사용 시 문제점

AsyncTask 취소

 - mayInterruptIfRunning의 의미

HandlerThread란?

deep sleep 현상은 무엇이고 이에 대한 대책은?

Context는 어떤 데 쓰이고 자식 클래스는 어떻게 되나?

Activity에서 this, getBaseContext(), getApplicationContext() 차이

ActivityA에서 ActivityB 호출한 경우 생명주기/ActivityA로 다시 돌아오는 경우 생명주기

onSaveInstanceState() 실행 시점

taskAffinity 동작

singleTask, singleInstance 차이

Fragment 쓰면 장점, 불편한 점

Fragment 정적 생성 메서드를 쓰는 이유

서비스는 언제 사용?

시스템 서비스와 서비스 컴포넌트 차이점

Service onStartCommand 리턴 값 구분

Started & Bound Service 언제 쓰이나?

BIND_AUTO_CREATE 옵션은 왜 쓰나?

IntentService

aidl 스텁과 프락시

서비스와 Activity 통신 방법은 어떤 것들이?

cp와 직접 db 접근 코드 선택 기준

db lock이란?

db transaction의 장점과 문제

BR에서 Toast를 띄우면?

LocalBroadcastManager 역할

Application의 용도

SharedPreferences commit()과 apply() 차이

google play service 연결은 어떻게 하는가? 내부 구조


Parcelable/Serializable 차이

메모리릭 확인하는 방법

targetSdkVersion 용도

싱글톤에 Context가 그대로 전달되면 어떤 문제가 생기는가? (코드로 만들기)


아래 코드의 실행 결과는?

for (int i = 0; i < 4; i++) {

 title.setText("current=" + i);

 SystemClock.sleep(1000);

}


======= 일반 =====

State 패턴과 Strategy 패턴 차이

마커 인터페이스

애너테이션 활용은?

AOP 장단점?

스레드풀 AbortPolicy

CountDownLatch 사용. 주의할 점

enum은 어떤 때 쓰는가?

for each 내부 구조는?

다이나믹 프록시는?

======== RxJava ===

Subject 문제점과 어느 때 사용하면 되는가?

Scheduler와 Worker의 관계

subscribeOn/observerOn 차이
```