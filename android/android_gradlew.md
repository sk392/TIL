## android_gradlew

Android Studio 왼쪽에 프로젝트 트리를 보면 gradlew라는 이름의 파일이 있다. [gradlew](!https://developer.android.com/studio/build/building-cmdline?hl=ko)는 Gradle Wrapper의 줄임말이고, Android프로젝트에서 사용 가능한 모든 빌드 작업을 실행할 수 있다.

 gradlew에서 할 수 있는 작업 중에 라이브러리 간에 디펜던시 체크할 수 있는 기능에 대해서 알아보자(사실은 요것밖에 아직 모름)
 
### Android에서 Library Dependencies 체크하기

단도직입적으로 해보자

terminal에서 ``` ./gradlew app:dependencies > text.txt
``` 요렇게 명령어를 쓰면 text.txt파일에 모든 디펜던시 트리가 나온다.


* './gradlew' : 리눅스에서 그냥 gradlew를 치면 usr/bin이나 bin에 있는 gradlew 파일을 실행시키라는 의미고, ./를 앞에 붙여주면 현재 폴더라는 의미다 (참고로 ../는 부모) 결국 현재 폴더의 gradlew라는 파일을 실행하라는 의미이다.
* 'app:' : Android Studio에 오른쪽에 보면 Gradle이라는 탭이 있는데 열어보면 루트 프로젝트와 함께 모듈들이 보인다. 그 중에 app이라는 모듈에 포함된 태스크를 실행시키라는 의미 app:이 없으면 루트 프로젝트의 태스크를 실행시킨다.
* 'dependencies' : 아까와 마찬가지로 오른쪽 Gradle에서 app > Tasks > help에 보면 다양한 명령어들이 보이는데, 그 중에 하나로서 이 명령어는 해당 모듈에서 포함하고 있는 디펜던시 트리에 대해 알려준다.
* ' > text.txt' : 결과를 텍스트파일에 넣는 기능으로 해당 명령어의 결과는 터미널 페이지에서 전부 확인할 수 없을 정도로 길기 때문에 파일에서 찾아서 쓰는게 편함!


### 결과를 읽어보자

해당 파일을 잘 내리다 보면(엄청길다) 아래와 같이 트리형태로 존재하는 것을 알 수 있는데, 오른쪽으로 뎁스가 길어지면 해당 라이브러리를 사용하고있어서 디펜던시에 걸린다는 의미로 읽으면 된다. 

* '(*)' : 위에서 해당 라이브러리에 대해 디펜던시 트리를 한번 설명했으니 현재 타임에선 스킵하겠다는 의미이다.
* '->' : 요 화살표는 해당 디펜던시 트리에선 1.0.0을 참고 하고 있는데, 다른 디펜던시에 의해서 1.1.0이 되었다는 뜻이다.

```
...


|    +--- androidx.appcompat:appcompat:1.0.0 -> 1.1.0
|    |    +--- androidx.annotation:annotation:1.1.0
|    |    +--- androidx.core:core:1.1.0
|    |    |    +--- androidx.annotation:annotation:1.1.0
|    |    |    +--- androidx.lifecycle:lifecycle-runtime:2.0.0 -> 2.1.0 (*)
|    |    |    +--- androidx.versionedparcelable:versionedparcelable:1.1.0
|    |    |    |    +--- androidx.annotation:annotation:1.1.0
|    |    |    |    \--- androidx.collection:collection:1.0.0 -> 1.1.0 (*)
|    |    |    \--- androidx.collection:collection:1.0.0 -> 1.1.0 (*)

...

```


### 트러블 슈팅

이번에 Androidx.appcompat:appcompat:1.1.0에서 특정 웹뷰 버전에서 죽는 이슈가 있었고, 해당 이슈를 해결하기 위해서 appcompat:1.1.0 디펜던시를 제거해야했는데, gradle에 버전을 1.1.0 -> 1.0.2로 낮춰도 androidx.preference 라이브러리에서 참조하고 있던 부분이 있어서 1.0.2로 다운그레이드가 되지 않았었다 해당 버전도 1.0.0으로 낮춰서 문제를 결국 해결!

