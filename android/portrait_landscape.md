## android Portrait_landscape 속성


안드로이드 형태별 표시 형식

portrait : 화면을 세로 방향으로 고정

landscape : 화면을 가로 방향으로 고정

sensorPortrait : 화면을 세로 방향으로 고정, 센서에 따라 정/역방향으로 표시

sensorLandscape : 화면을 세로 방향으로 고정, 센서에 따라 정/역방향으로 표시

sensor : 센서에 따라 최대 4가지 방향으로 회전

코드로 레이아웃을 관리하는 경우, Activity에서 onConfigurationChanged 메소드를 통해 방향이 바뀌었을 때의 처리를 해줄 수 있습니다.

하지만 XML으로만 레이아웃을 관리하는 경우 위 메소드는 필요 없습니다.

기본적으로 가로모드일때의 레이아웃을 다르게 설정하면 좋진 않습니다.

왜냐하면 레이아웃을 다르게 줘야하기 때문에 레이아웃이 변경되면서 저장된 데이터가 날아가게됩니다.

따라서 데이터를 그대로 유지시키기 위한 코딩이 필요합니다.



출처: http://cocomo.tistory.com/417 [Cocomo Coding]