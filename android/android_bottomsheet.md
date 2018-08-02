## BottomSheet

#### 사용법

BottomSheet는 별도의 위젯으로 존재 하지는 않는다. CoordinatorLayout을 이용하여 자식뷰들의 행동을 구현한것으로 속성변경만으로 간단하게 BottomSheet를 사용 할 수 있다.

* appbar_scrolling_view_behavior
– android.support.design.widget.AppBarLayout$ScrollingViewBehavior

* bottom_sheet_behavior
– android.support.design.widget.BottomSheetBehavior


CoordinatorLayout을 사용해본 적이 있다면 레이아웃에 layout_behavior속성을 사용해본 적이 있을 것이다. 속성의 값으로 appbar_scrolling_view_behavior를 사용한적이 있을 것이다. 이는 툴바와 스크롤 되는 뷰간의 상호 작용을 위해 구현된 behavior이다. BottomSheet또한 CoordinatorLayout의 하나의 behavior이며, bottom_sheet_behavior를 사용하면 된다. bottom_sheet_behavior는 스트링 이름이며 값은 클래스명으로 지정되어 있으며 해당 클래스가 로드되어 수행되는 구조이다.

```

<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@android:color/white">

    <LinearLayout
        android:id="@+id/bottomSheet"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="#EEE"
        android:orientation="vertical"
        app:behavior_peekHeight="64dp"
        app:behavior_hideable="false"
        app:layout_behavior="@string/bottom_sheet_behavior">

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:padding="16dp"
            android:textColor="@color/colorPrimaryDark"
            android:textAppearance="?android:attr/textAppearanceLarge"
            android:text="Bottom sheets" />


        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:padding="16dp"
            android:textAppearance="?android:attr/textAppearanceMedium"
            android:text="Bottom sheets slide up from the bottom edge of the screen to reveal additional content."/>


        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:padding="16dp"
            android:layout_marginTop="8dp"
            android:textAppearance="?android:attr/textAppearanceSmall"
            android:text="Modal bottom sheets are alternatives to menus, or simple dialogs, and can display deep-linked content from another app. They appear above other UI elements and must be dismissed in order to interact with the underlying content. When a modal bottom sheet slides into the screen, the rest of the screen dims, giving focus to the bottom sheet." />

    </LinearLayout>

    <android.support.design.widget.FloatingActionButton
        android:id="@+id/fab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginRight="16dp"
        android:layout_marginEnd="16dp"
        android:src="@mipmap/ic_launcher"
        app:layout_anchor="@+id/bottomSheet"
        app:layout_anchorGravity="top|right" />

</android.support.design.widget.CoordinatorLayout>
```


CoordinatorLayout 자식뷰에서 BottomSheet를 사용하고 싶은 레이아웃에 behavior속성을 주면된다.  값에는 이미 정의된 @string/bottom_sheet_behavior값을 주면된다. 정의된 값은 라이브러리에 기본적으로 포함되어 있으며, 나타나지 않는 다면 AppCompat V23.2인지 확인해봐야 한다.

BottomSheet높이는 자식뷰의 크기에 따라 변하게 되며 기본적으로 보여지게 될 높이는 behavior_peekHeight 속성을 통해 지정 할 수 있다. behavior_peekHeight[dpi] 값은 반드시 필요하며 값을 주지 않는 경우 자식뷰의 크기계산을 하지 못하기 때문에 뷰가 나타나지 않을 수 있다. 그리고 behavior_peekHeight 속성을 준 높이보다 더 최소한으로 사용자가 감출 수 있도록  behavior_hideable[true/false] 속성을 지원한다.

* behavior_peekHeight: 기본적으로 보여질 높이

* behavior_hideable: 사용자의 액션에 의해 완전지 감춰질지 여부