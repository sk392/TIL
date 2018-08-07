## android TextView의 em

TextView의 너비를 EM 단위의 크기로 설정하기 위해 "ems" 속성을 사용합니다. EM("em")이라는 단위는 CSS(Cascading Style Sheets)를 다뤄본 사람이라면 한번 쯤은 접해본 경험이 있을 것입니다.

* android:ems - TextView의 너비를 EM 단위 값으로 지정.
	* 정수 값 사용. (예. 5)
	* 현재 폰트에 따른 EM 단위 값으로 TextView의 너비가 고정됨.
	* layout_width가 반드시 "wrap_content" 이어야 함.
	* 다른 방법(width 속성 등)으로 TextView의 크기를 지정하면 ems 값은 무시됨.

	
#### EM
EM은 활자인쇄(Typography) 분야에서 사용하는 단위인데, 현재 디지털 영역에서는 폰트의 pt(point) 크기를 나타내는 값입니다. 만약, 16pt 폰트를 사용하고 있다면, 1em은 16pt를 나타내는 것이죠.


[wiki - em (typography)]에 따르면, (아래에 발췌한) 여러 가지 이유들로 인해, 현대의 활자체들에서 'M'문자는 1em보다 다소 작은 너비를 가진다고 합니다. 특히 디지털에서의 EM은 'M' 문자의 너비와 관계없이, 항상 폰트가 가진 point size(pt)를 의미한다고 합니다.


#### EM in Android TextView

자, 그럼 EM 단위의 크기로 TextView의 너비(width)를 설정하는 "ems" 속성은 언제 사용하는 걸까요? 바로, TextView에 적용되는 폰트 크기에 따라 TextView의 너비가 항상 동일한 비율의 너비를 가지도록 만들 때 사용될 수 있습니다.


즉, TextView의 폰트 크기가 바뀌어도, 동일한 텍스트에 대해, TextView 내에서 항상 같은 모양으로 표시되도록 만들고자 할 때 유용하게 사용할 수 있습니다. 폰트 크기에 따라 크기 비율을 조절한 것 처럼 말이죠.



아래의 예제를 통해 ems 속성이 어떻게 동작하는지 확인할 수 있습니다.
```
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/text1"
        android:background="#FF0000"
        android:ems="10"
        android:text="ABCDE12345가나다라마"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/text2"
        android:layout_below="@id/text1"
        android:background="#00FF00"
        android:textSize="24sp"
        android:ems="10"
        android:text="ABCDE12345가나다라마"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/text3"
        android:layout_below="@id/text2"
        android:background="#0000FF"
        android:textColor="#FFFFFF"
        android:textSize="36sp"
        android:ems="10"
        android:text="ABCDE12345가나다라마"/>
```
