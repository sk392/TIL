## BroadCast 정리

* Global Broadcast : 일반적으로 이야기하는 Broadcast. 프로세스 경계를 무시하고 안드로이드 시스템 상에 등록된 모든 Receiver 들에게 전달되는 Broadcast 이다. 보통 안드로이드 시스템 메시지를 수신하고, 이에 따른 동작을 정의하기 위해 사용하지만 Remote Service 에서 Activity 에 이벤트를 알릴때와 같은 프로세스의 벽을 넘어야 하는 경우에도 유용하다.  

 

* Local Broadcast : 현재 프로세스 안에서만 유효한 Broadcast 이다. Global Broadcast 에 비해 훨씬 시스템 부하가 적다. 액티비티 내부의 객체간에 상호 의존성을 낮추어 깔끔한 프로그램 구조를 만들 수 있다는 점이 장점이다.


##### 예제코드

```
@Override

protected void onCreate(Bundle savedInstanceState) {

super.onCreate(savedInstanceState);

setContentView(R.layout.activity_main);


//Intent Filter

IntentFilter filter = new IntentFilter();

filter.addAction(BROADCAST_LOCAL_TEST);


//regi

LocalBroadcastManager.getInstance(this).registerReceiver(mReceiver, filter);


//btn

Button btn = (Button)findViewById(R.id.send_btn);

btn.setOnClickListener(new OnClickListener() {


@Override

public void onClick(View v) {

//create intent

Intent intent = new Intent(BROADCAST_LOCAL_TEST);

LocalBroadcastManager.getInstance(MainActivity.this).sendBroadcast(intent);

}
});
}


@Override

protected void onDestroy() {

super.onDestroy();

//unregi

LocalBroadcastManager.getInstance(this).unregisterReceiver(mReceiver);

}
```