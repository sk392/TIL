## JVM

#### JVM의 구조

실행될 클래스 파일을 메모리에 로드 후 초기화 작업 수행

메소드와 클래스변수들을 해당 메모리 영역애 배치

클래스로드가 끝난 후 JVM은 main 메소드를 찾아 지역변수, 객체변수, 참조변수를 스택에 쌓음
다음 라인을 진행하면서 상황에 맞는 작업 수행(함수 호출, 객체 할당 등)

![JVM구조](https://t1.daumcdn.net/cfile/tistory/2540294C5654207F26)

Class Loader: JVM내로 클래스를 로드하고 링크를 통해 배치하는 작업을 수행하는 모듈로써 런타임시 동적으로 클래스를 로드한다.

Execution Engine: Class Loader를 통해 JVM 내의 런타임 데이터 영역에 배치된 바이트 코드를 실행한다. 이 때, 
Execution Engine은 자바 바이트 코드를 명령어 단위로 읽어서 실행한다.

Garbage Collector: JVM은 Garbage Collector를 통해 메모리 관리 기능을 자동으로 수행한다. 애플리케이션이 생성한 객체의 생존 여부를 판단하여 더 이상 사용되지 않는 객체를 해제하는 방식으로 메모리를 자동 관리한다.

Runtime Data Areas: JVM이 운영체제 위에서 실행되면서 할당받는 메모리 영역이다. Class Loader에서 준비한 데이터들을 보관하는 저장소이다.
