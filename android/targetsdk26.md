## TargetSdk26(8.0.0)으로 올려야하는 이유

자세한 문서는 [여기](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html)를 참조해주세요!

#### 2018 년 말의 타겟 API 수준 26으로 상향 요구 사항

1. bindService()의 암시적 인텐트는 더이상 지원하지 않음 ([Android 5.0](https://developer.android.com/about/versions/android-5.0-changes#BindService))
2. 런타임 권한 (통화, 기기저장소 등등..) ([Android 6.0](https://developer.android.com/about/versions/marshmallow/android-6.0-changes#behavior-runtime-permissions))
3. 시스템에서 제공되는 인증서만을 신뢰하며 사용자가 추가한 인증 기관(CA)은 더 이상 신뢰하지 않는다. ([Android 7.0](https://developer.android.com/about/versions/nougat/android-7.0#default_trusted_ca))
4. 사용자가 계정 액세스 권한을 허용하지 않은 경우에는 앱이 더 이상 사용자 계정에 액세스할 수 없다 ([Android 8.0](https://developer.android.com/about/versions/oreo/android-8.0-changes#aaad))

위와 같이 타겟 SDK버전을 올리는 가장 큰 이유는 보안 때문이다. 자세한 한글 문서는 다음과 같다.

API 동작 변경으로 인해 Android의 보안 및 개인 정보 보호가 향상되어 개발자가 앱을 보호하고 사람들을 악성 코드로부터 보호 할 수 있습니다. 다음은 최근 플랫폼 버전의 몇 가지 변경 사항입니다.

이러한 변경 사항 중 대부분은 targetSdkVersion 매니페스트 속성을 통해 새로운 API 동작에 대한 지원을 명시 적으로 선언 한 앱에만 적용됩니다 . 예를 들어, targetSdkVersion23 (API 레벨이 안드로이드 6.0) 이상인 앱만이 앱이 런타임 권한을 통해 액세스 할 수있는 개인 데이터 (예 : 연락처 또는 위치)를 완전히 제어 할 수 있습니다. 마찬가지로 최신 버전에는 배터리 및 메모리와 같은 리소스가 실수로 과도하게 사용되는 것을 방지하는 사용자 환경 개선 기능이 포함되어 있습니다. 백그라운드 실행 제한 은 이러한 유형의 개선에 대한 좋은 예입니다.

사용자에게 가능한 최고의 Android 환경을 제공하기 위해 Google Play Console에서는 앱이 최신 API 수준을 타겟팅하도록 요구합니다.

2018 년 8 월 : API 레벨 26 (Android 8.0) 이상을 타겟팅하려면 새로운 앱이 필요합니다.
2018 년 11 월 : API 레벨 26 이상을 타겟팅하는 데 필요한 기존 앱을 업데이트합니다.
2019 년 이후 : 매년 targetSdkVersion요구 사항이 향상 될 것입니다. Android 디저트 출시 이후 1 년 이내에 새로운 앱 및 앱 업데이트는 해당 API 수준 이상을 타겟팅해야합니다.
업데이트를 수신하지 않는 기존 앱은 영향을받지 않습니다. 개발자는 원하는대로 자유롭게 사용할 수 minSdkVersion 있으므로 이전 Android 버전 용 앱을 개발할 수있는 능력에는 변화가 없습니다. 개발자는 합리적으로 가능한 한 하위 호환성을 제공 할 것을 권장합니다. 향후 Android 버전에서는 최근 API 수준을 목표로하지 않으며 성능이나 보안에 부정적인 영향을 미치는 앱도 제한합니다. Google은 앱 생태계의 분열을 사전에 방지하고 앱의 안전성과 성능을 보장하면서 개발자에게 긴 창을 제공하고 미리 계획을 세우는 데 필요한 많은주의 사항을 제공하고자합니다.

올해 안드로이드의 가장 안전하고 최고의 성능을 자랑하는 안드로이드 오레오 (Android Oreo)를 출시했습니다. 우리는 Project Treble 을 도입 하여 최신 버전의 장치가 더 빨리 출시되도록 도와줍니다. Android 8.1 Oreo 를 대상으로하는 앱을 지금 시작하십시오.