## Dumpsys


안드로이드를 개발하다보면 내가 만든 activity나 fragment를 확인하기 육안이나 로그로 확인하기 어려울 때가 있다. 현재 내 앱의 시스템 상태를 확인할 수 있다.

### 사용예시

터미널에 아래와 같이 입력하면 된다.

```
//명령어 
adb shell dumpsys activity ${packageName}
```

아래와 같이 현재 떠있는 activity나 fragment를 확인할 수 있고 View Hierarchy도 함께 확인 할 수 있다.

```
TASK com.kakao.talk id=595 userId=0
  ACTIVITY com.kakao.talk/.activity.main.MainTabFragmentActivity 451df53 pid=23007
    Local Activity b5337ad State:
      mResumed=false mStopped=true mFinished=false
      mChangingConfigurations=false
      mCurrentConfig={0 1.1 themeSeq = 0 showBtnBg = 0 450mcc6mnc [ko_KR] ldltr sw411dp w411dp h773dp 420dpi nrml long hdr port finger -keyb/v/h -nav/h appBounds=Rect(0, 0 - 1080, 2094) s.45 mkbd/h desktop/d ?dc}
      threadConfig={0 1.1 themeSeq = 0 showBtnBg = 0 450mcc6mnc [ko_KR] ldltr sw411dp w411dp h773dp 420dpi nrml long hdr port finger -keyb/v/h -nav/h appBounds=Rect(0, 0 - 1080, 2094) s.45 mkbd/h desktop/d ?dc}  isDexCompatMode=false
      mLoadersStarted=true
      Active Fragments in 2fd1a78:
      ...  
      
      View Hierarchy:
      DecorView@e88d633[MainTabFragmentActivity]
        android.widget.LinearLayout{e3a58d V.E...... ......ID 0,0-1080,2094}
          android.view.ViewStub{56ca342 G.E...... ......I. 0,0-0,0 #10201c4 android:id/action_mode_bar_stub}
          android.widget.FrameLayout{bcf53 V.E...... ......ID 0,63-1080,2094}
            androidx.appcompat.widget.FitWindowsLinearLayout{b64c090 V.E...... ......ID 0,0-1080,2031 #7f09001f app:id/action_bar_root}
              androidx.appcompat.widget.ViewStubCompat{bf20d89 G.E...... ......I. 0,0-0,0 #7f09003b app:id/action_mode_bar_stub}
              androidx.appcompat.widget.ContentFrameLayout{725b18e V.E...... ......ID 0,0-1080,2031 #1020002 android:id/content}
                android.widget.RelativeLayout{8fa0daf V.E...... ......ID 0,0-1080,2031}
                  androidx.coordinatorlayout.widget.CoordinatorLayout{6fd55bc V.E...... ......ID 0,0-1080,2031}
                    com.kakao.talk.activity.main.MainTabAppBarLayout{bbf4fb4 V.E...... ......ID 0,0-1080,0}
                      com.kakao.talk.widget.theme.ThemeToolBar{564f945 G.E...... ......ID 0,0-0,0 #7f091401 app:id/toolbar}
                        com.kakao.talk.widget.theme.ThemeRelativeLayout{4f589a V.E...... ......I. 0,0-0,0 #7f09140d app:id/toolbar_main_container}
                        ... 
```