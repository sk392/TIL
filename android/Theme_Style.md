## Android에서 프로그래밍적으로 Theme과 Style 접근하기

안드로이드는 theme과 style을 제공한다. 나름 html의 css와 비슷하게 앱 내부에서 일관된 UI를 손쉽게 제공하는 용도로 사용할 수 있는데, 이게 참 거시기한 부분이 많다.

1. 대체 theme의 이 속성이 어떤 역할을 하는지 감이 잘 안온다. android 시스템의 theme.xml 을 열어보면 속성만 수십개가 나온다. 봐선 한번에 이해가 가는 녀석들도 있지만 dropdown 관련된 것만 해도 몇개나 된다. 관련 문서는 전무하다. 다 소스를 뒤져보라는 것인지.
2. 프로그램적으로 다루기 쉽지 않다. 뭔가 theme의 속성을 프로그램적으로 적용하려고 한다거나, default style을 적용한다거나, style을 프로그램적으로 적용해보려면 꽤나 삽질을 해야 한다.

1번이야 별 수가 없이 시행착오를 거쳐야 한다. 그나마 요즘 intelliJ나 android studio의 preview가 워낙 좋아졌기에 이런 부분은 바로바로 확인 가능하다. 또한, actionbarsherlock이나 holoeverwhere 등 theme을 아주 헤비하게 뜯어고친 라이브러리들을 보면서 비교분석해 보면 삽질을 좀 줄일 수 있을 것이다.

오늘 할 얘기는 2번인데, 프로그램적으로 style/theme을 다룰 때 생기는 이슈 중 내가 알아낸 내용만 간단히 정리해보려고 한다.

##### 현재 theme의 속성 resource id 가져오기

현재 theme의 windowTitleStyle 속성의 resouce id를 가져와보자. xml로 따지자면 ?attr:windowTitleStyle 에 해당한다.

```
TypedValue outValue = new TypedValue();
getContext().getTheme().resolveAttribute(android.R.attr.windowTitleStyle, outValue, true);
int resId = outValue.resouceId; 
```

##### TextAppearance.DialogWindowTitle 라는 style의 textSize 와 textColor를 가져와보자.

```
TypedArray a = getContext().getTheme().obtainStyledAttributes( android.R.style.TextAppearance_DialogWindowTitle, new int[]{android.R.attr.textSize,android.R.attr.textColor});
int textSize = a.getDimensionPixelSize(0,0);
int textColor = a.getColor(1,0);
a.recycle();
```

이런 형태는 커스텀 뷰의 커스틈 xml 속성 가져오기에서 많이 해 봤을 것이다. a.getXXX 메서드를 호출할 때 인자로 넘기는 인덱스 값은 new []{} 에서 넘긴 속성의 인덱스이다. 위에선 android.R.attr.textColor 의 배열 상 인덱스가 1이므로 1을 넘겼다. a.recycle() 까먹지 말고 반드시 호출하자!

##### 커스텀 뷰에 theme에서 정의한 기본 style을 적용하기
안드로이드 UI 작업을 하다보면 커스텀 뷰를 만들 일이 꽤 많다. 그런데 매번 커스텀 뷰의 style을 적용하기 귀찮아서 default style을 theme에 만들어두고 사용하고자 한다면 어떻게 할까? (결국 이게 theme의 목적이겠지)

우선 theme에 커스텀 속성을 하나 만들어둬야 한다. theme에 커스텀 속성을 추가하는 방법은 커스텀 뷰에 커스텀 속성을 추가하는 방법과 동일하다. values/ 에 attribute 정의하는 xml을 하나 만들고 거기 아무 이름이나 styleable을 하나 만들어 둔 다음 attr을 정의하면 된다.

```
<declare-styleable
 name="CustomTheme"
>

    
<attr
 name="myCustomView" format="reference"
/>

</declare-styleable>
```

저기 입력한 CustomTheme 이란 값은 xml이나 java코드에서 사용할 일 없다.

theme에선 이렇게 사용하면 된다.

```
<style
 name="MyAppTheme" parent="@style/Theme"
>

    
<item
 name="myCustomView"
>
@style/myCustomViewDefaultStyle
</item>

</style>
```

당연히 myCustomViewDefaultStyle도 일반적인 스타일 만들듯이 등록해둬야 한다.

자, 이제 MyImageView 라는 커스텀 뷰에 myCustomViewDefaultStyle 을 적용할 차례다.

```
public MyImageView(Context context, AttributeSet attrs) {
    super(context, attrs, R.attr.myCustomView);
}
```

##### 커스텀 뷰에 커스텀 속성 적용하기

이 부분은 많이들 해 봤을 것이다. 이건 딱히 스타일이랑 관련은 없으니… 바로 위의 절과 연관지어서, 커스텀 뷰에 theme의 기본 스타일을 적용하고, 이 기본 스타일에 정의된 커스텀 속성을 적용하는 방법까지 엮어서 만들면 다음과 같다.

```
public MyImageView(Context context, AttributeSet attrs) {
    super(context, attrs, R.attr.myCustomView);
    init(attrs, R.attr.myCustomView);
}

private void init(AttributeSet attrs, int defStyle) {
   TypedArray a = getContext().getTheme().obtainStyledAttributes(attrs, R.styleable.MyImageView, defStyle, 0);
   //blah blah
   a.recycle();
}
```

여기서 init() 메서드의 두번째 인자로 R.attr.myCustomView 를 넘긴 것을 눈여겨보자. 이렇게 하면 xml상에서 MyImageView를 선언했을 때 속성을 별도로 명시하지 않았을 때 theme의 myCustomView이 참조하는 스타일의 해당 속성이 적용된다. 만약 저 defStyle 부분을 0으로 해 버리면 아무리 theme에 myCustomView 속성을 명시해 봤자, super의 속성에만 기본 스타일 값이 적용되고, init() 메서드 안에서 조작하고자 하는 녀석들엔 기본 스타일 값 (위 예제에선 myCustomViewDefaultStyle 라는 스타일의 값)이 적용되지 않는다.

추가적인 사항은 [여기](http://kingorihouse.tumblr.com/post/72754091707/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C%EC%97%90%EC%84%9C-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8%EC%A0%81%EC%9C%BC%EB%A1%9C-theme%EC%99%80-style-%EC%A0%91%EA%B7%BC%ED%95%98%EA%B8%B0)를 참고.