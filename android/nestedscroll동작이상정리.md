## 같은 FrameLayout안에 2개 이상의 NestedScroll이 가능한 뷰가 있을 때 동작이상.


###레이아웃 구조 예시
```
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout 
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <android.support.design.widget.AppBarLayout
        android:id="@+id/appbar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <android.support.design.widget.CollapsingToolbarLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:layout_scrollFlags="scroll|snap|enterAlways">
            <!--something-->
        </android.support.design.widget.CollapsingToolbarLayout>
    </android.support.design.widget.AppBarLayout>


    <FrameLayout
        android:id="@+id/content_layout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior">


        <android.support.v7.widget.RecyclerView
            android:id="@+id/list_a"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />

        <android.support.v7.widget.RecyclerView
            android:id="@+id/list_b"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />

    </FrameLayout>


</android.support.design.widget.CoordinatorLayout>



```

1. 사전정보
    1. 스크롤은 Drag와 Fling으로 나뉘어짐.
    2. Drag는 Touch / Fling은 Non_Touch
2. 상태
    1. coordinatorLayout 밑에 있는 같은 프레임 레이아웃에서 2개 이상의 Recyclerview(NestedScrollEnable View)가 존재
    2. 탭 전환하며 스크롤하는 경우 (fling 상태) coordinatorLayout의 onNestedPreScroll이 불리지 않음.
    3. Drag는 정상 동작하나 Fling은 동작하지 않음.
3. 원인
    1. CoordinatorLayout은 2개의 차일드를 가지고 있는데 Appbar랑 frameLayout.
    2.  framelayout안에 2개의 타겟인 Recyclerview가 있다. 각 A,B라고 칭한다.
    3. 스크롤이 발생하는 순서는 아래와 같다.
        1. Drag - type Touch
            1. StartNestedScroll [accepted : true]
            2. StopNestedScroll [accepted : false]
        2. Fling - type: NonTouch
            1. StartNestedScroll [accepted : true]
            2. StopNestedScroll [accepted : false]
    4. A에서 스크롤을 마치고 B로 전환되었을 때, Fling이 들어가지 않는 시점이 있는데,
        1. (A)Drag - 1 --> (A)Drag - 2 --> (A)Fling - 1 --> (B)Drag - 1 --> (B)Drag - 2 --> (B)Fling - 1 --> (A)Fling - 2 --> (B)Fing - 2
    5. A의 fling이 나는 시점에서 Non Touch에 대한 accepted가 false로 들어가게 된다.
    6. 이렇게 되면 B에서 Fling을 하던 말던 accepted가 false이기 때문에 Fling이 들어가지 않게 된당.
        1. if (hasNestedScrollingParent(type)) - 요라인
    7. 그래서 Behavior에 [type NonTouch]onNestedPreScroll이 불리지 않게 된다.
    8. 이렇게 hasNestedScrollingParent[accepted]가 공유 되는 이유는 Coordinatorlayout아래의 하나의 뷰그룹에 묶여 있기 때문이다. 해당 값은 FrameLayout에서 동작하므로
4. 해결방안
    1. behavior에서 onNestStartScroll이 불릴 경우에 타입별로 해당 값을 저장해서 가지고 있고
    2. 다시 스타트로 값이 들어올 경우에 이전 값과 비교해서 다른 뷰에서 값을 가져온다면
    3. 이전 뷰를 스탑을 넣어주고
    4. stop이 된경우에는 타입별로 저장한 값을 날린다.