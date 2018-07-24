## Android 키 입력 전달 메커니즘


1. 기본 내용은 커널에서 네이티브 단을 거쳐서 앱에 입력 이벤트가 전달되는 것

2. InputReader에서 EventHub를 통해 커널에서 이벤트를 가져오고 
InputDispatcher는 이벤트를 전달한다

3. 이제 앱에서 이벤트를 전달받는데, Activity는 Window(구체적으로 PhoneWindow)를 갖고 Window는 ViewRootImpl과 일대일 매핑된다


4. ViewRootImpl의 내부 클래스인 WindowInputReventReceiver에서 이벤트를 전달받아서 하위 ViewGroup/View로 전달한다

5. WindowInputEventReceiver에서 전달받은 파라미터는 InputEvent로 MotionEvent와 KeyEvent의 상위 추상 클래스이다

6. enQueueInputEvent에서 메시지를 날림.
7. 핸들러에 메시지 보냄
8. 받아서 dispatchKeyEvent..? 여긴 뇌피셜