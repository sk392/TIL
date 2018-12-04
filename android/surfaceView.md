## SurfaceView

일반 View는 onDraw 메소드를 시스템에서 자동으로 호출해줌으로써 화면을 그린다. 그래서 화면에 늦게 그려질 수도 있다.

SurfaceView는 그리기를 시스템에 맡기는 것이 아니라 스레드를 이용해 강제로 화면에 그림으로써 원하는 시점에 바로 화면에 그릴 수 있다.

그래서 SurfaceView는 애니메이션이나 동영상과 같이 연산처리가 많이 필요한 뷰를 위해 사용된다.

SurfaceView는 더블 버퍼링 기법을 이용하여 SurfaceHolder가 Surface에 미리 그리고 이 Surface가 SurfaceView에 반영되는 방식이다.



SurfaceView는 자기 영역 부분의 Window를 뚫어서(punch) 자신이 보여지게끔 하고 Window와 View가 블렌딩되어 화면에 보여지게 된다.

![SurfaceView](http://postfiles6.naver.net/MjAxNzA3MThfMzMg/MDAxNTAwMzY4NTYzMTUz.MwDn2xyuHAe-2A86x42JYN5gT6Oo8XzAT3Qtm4vSJcUg.cM-ZCdjioFj_9osza-Hk8M_ZBp6jedgyj_MkxaKdaNMg.PNG.muri1004/surfaceview.png?type=w2)