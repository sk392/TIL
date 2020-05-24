## Android WebView

Android WebView는 브라우저를 제공하는 앱서비스에서 굉장히 중요한 부분을 차지하고 있다. 잘못된 웹뷰로 인해 크래시로그가 굉장히 많이 올라간다던가 웹뷰에서 특정 기능이 Deprecated되거나 추가되는 것들에대해서 어느정도 인지하고 있어야한다.


### Chrome ReleaseNote

[Chrome ReleaseNote](https://chromereleases.googleblog.com/)는 각종 Chrome Browser나 WebView, OS에 대한 릴리즈노트를 적어 둔 곳이다.

Dev나 Beta, Stable에 대한 모든 배포가 작성되며 자주 사용되진 않지만 해당 배포에 대한 Git Log도 확인할 수 있다.


### Chrome Source

Chrome [GitHub](hhttps://github.com/chromium/chromium/tree/84.0.4135.2)에 들어가면 특정 버전의 코드를 볼 수 있는데, 여긴 아직 미지의 세계이지만 특정 버전의 Feature를 확인할 수 있는 방법은 있다. 특정 버전의 소스코드에 [SupportLibWebViewChromiumFactory.java](android_webview/support_library/java/src/org/chromium/support_lib_glue/SupportLibWebViewChromiumFactory.java)에 들어가면 mWebViewSupportedFeatures에 값을 볼 수 있는데 여기서 지원하는 feature에 대해서 알 수 있다. ~해당 지원되는 feature에 대한 매핑은 아직 잘 모르겠다~

그리고 가장 보기 좋은 [ReleaseNote](https://www.chromestatus.com/features/schedule)는 여기를 보면된다


### Etc..

1. android 4.4이하에서는 웹뷰를 업그레이드할 수 없고 OS배포에 포함된 WebView만 사용가능하다. 5.0 이상부터는 Google Play Store를 통해 최신 웹뷰를 사용할 수 있다.
2. android sourceCode를 검색할 때는 [여기](https://cs.android.com/)에서 검색하면 된다.