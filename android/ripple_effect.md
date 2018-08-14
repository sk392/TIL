## Ripple Effect 


안드로이드에서 버튼을 클릭 및 터치 시에 파동처럼 퍼져나가는 이펙트를 Ripple Effect라고 한다.

이러한 리플 이펙트도 커스텀이 가능한데 총 2가지 방법이 있다.
 
1 . 아래의 내용을 이벤트를 주고자하는 view xml에 넣는다.

```
android:clickable="true"
android:background="?attr/selectableItemBackground"
```

2 . 다음과 같이 drawble forder 내에 ripple_effect.xml 파일을 만들어(이름은 마음대로 작명) effect 효과를 커스텀 하여 넣는 것이다.

ripple_effect.xml 예제 소스는 다음과 같다


```
<?xml version="1.0" encoding="utf-8"?>
<ripple xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:color="@color/calcu_ripple_background"
    tools:targetApi="lollipop"> <!-- ripple effect color -->
    <item android:id="@android:id/background">
        <shape android:shape="rectangle">
            <solid android:color="@color/calcu_numbers_background" /> <!-- background color -->
        </shape>
    </item>
</ripple>
```

원하는 색상과 모양 타겟버전 등을 커스텀 할수 있다.

```
android:background="@drawable/ripple_effect"
```


백그라운드 값을 커스텀한 값으로 변경해주면 완성!

단 이 때 보면 알 수 있지만, ripple_effect는 백그라운드에 들어가므로 백그라운드에 어떤 이미지나 컬러가 들어간 경우는 리플이 동작하지 않는다.