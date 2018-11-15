## Android MediaPlayer


안드로이드에서 멀티미디어를 재생하는 방법중 하나인 MediaPlayer. 그중에서 안드로이드의 멀티미디어 아키텍처를 바탕으로, 오디오와 비디오 재생을 모두 담당하는 기본 API인 MediaPlayer로 안드로이드에서 비디오를 재생하는 방법을 정리해보자! 간단한 구조는 다음과 같다. 

![간단구조](https://images.contentful.com/s72atsk5w5jo/2FX1B5FFhKEIuOyYkAUukw/a29653854f7078b59e79b9942878f611/android-multimedia-architecture.png)

#### MediaPlayer의 상태 다이어 그램

미디어 플레이어에는 많은 상태가 있는데, 특정 상태에서만 사용할 수 있는 메서드들도 있으므로 조심해야된다. 말로 설명하는 것보다 도표가 훨씬 보기 좋으니 도표를 보자 ( [공식 홈페이지](!https://developer.android.com/reference/android/media/MediaPlayer?hl=ko#StateDiagram)엔 믿을 수 없이 저퀄?인 도표가 있다 )

![MediaPlayerState](https://media.springernature.com/lw785/springer-static/image/chp%3A10.1007%2F978-1-4302-5747-9_14/MediaObjects/978-1-4302-5747-9_14_Fig1_HTML.jpg)


#### SurfaceView

MediaPlayer에는 SrfaceView가 존재하는데, 메인스레드에서 직접 제어해주는 캔버스와 달리 백그라운드 스레드에서 화면을 업데이트 할 수 있어서 ANR(Application Not Responding)을 방지할 수 있다. 또, 사용자의 입력에 블럭없이 바로 반응할 수 있다. 이런 SurfaceView를 만들고 MediaPlayer에 지정해줘야 비로소 비디오를 재생할 수 있고. SurfaceView를 담고 있는 SurfaceHodler를 setDisplay()로 전달해줘야 한다.


#### 주의사항

1. 각 스테이트별로 호출 할 수 있는 메소드가 정해져있으니 잘 못호출하면 큰 일나는 것임.
2. initialized상태에서 호출할 수 있는 prepare()는 데이터를 가져오며 컨버팅하는 작업인데, 메인 스레드에서는 절대 호출하면 안됨. 네버에버 ANR이 발생할 수 있음
3. 릴리즈를 잘해줘야댐 MediaPlayer는 많은 시스템 리소스를 가지고 있기 때문에 Activity가 백그라운드에 나가 있을 때 릴리즈 해주고 resumed, restarted할 땐 새로 만들어주는게 좋다!