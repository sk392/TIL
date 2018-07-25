## Android Wakelock

#### 1. Permission 선언

제일 먼저 해야할 것은 permission을 선언하는 것입니다.

너무도 다행스러운 것은 동적 permission의 대상은 아니라는 것이네요.

아래와 같이 manifest에 선언해주면 됩니다.


#### 2. Cpu 깨어있도록 하기

사용자의 휴대폰이 Sleep모드로 들어가도, 폰의 CPU가 깨어있다면 일정한 시간에 계산을 해서,

화면을 켜고 소리를 플레이 하는 등의 과업을 수행할 수 있습니다.

이것은 어떻게 하는 것일까요?

Cpu가 깨어있도록 하는 것은,  안드로이드 시스템의 파워매니저로부터 wakeLock을 얻어와야 합니다.

따라서, 파워 매니저를 얻어오기 위해,  getSystemService()메소드를 사용하구요.

얻어온 파워매니저 객체에, newWakeLock 메소드를 사용해서 WakeLock객체를 얻어옵니다.

이 때 첫번째 인자로, WakeLock Flag를 받는데요.

어떤 wakeLock모드를 사용할 것인가에 대해서 선택하라는 것이구요.

여기에 사용할 수 있는 주요 Flag들은  아래와 같습니다.

PARTIAL_WAKE_LOCK 
CPU가 깨어 있도록 하는 것으로, 화면과 키보드 백라이트는 꺼집니다.
유저가 파워버튼을 누르면, 화면은 꺼지지만, CPU는 wakeLock이 release되기 전까지 계속 켜져 있습니다.
ACQUIRE_CAUSES_WAKEUP
WakeLock을 얻게 되었을 때, 화면을 보여지도록 하는 FLAG입니다.
ON_AFTER_RELEASE
WakeLock이 release되었을 때, 화면이 좀 더 켜져있도록 합니다.
(참고로 deprecated된, FULL_WAKE_LOCK Flag등은 제외 하였습니다.

공식문서에서는 WindowManager.LayoutParams.FLAG_KEEP_SCREEN_ON을 사용할 것을 권장하고 있네요)



아래코드와 같이 구현할 수 있는데요. 위에서 얻어온 wakeLock객체에 acquire()메소드를 사용해서, WakeLock을 얻어오면 됩니다.

#### 3. WakeLock Release


WakeLock을 얻어서  Cpu가 깨어있도록 하는 것은 배터리를 소모하는 일입니다.
따라서 원하는 시간에 과업을 마무리 했다면, release 해주어서 배터리소모를 줄여야 하는데요.

release 하기 위해서, wakeLock객체에 release()메소드를 사용해주면 됩니다.


```java

PowerManager powerManager = (PowerManager) context.getSystemService(Context.POWER_SERVICE);
PowerManager.WakeLock wl = powerManager.newWakeLock(WaklockFlag, "pushLibrary tag");
wl.acquire(10000);
```