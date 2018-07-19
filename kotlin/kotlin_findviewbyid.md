## FindViewById


API 레벨 26이상은 사용 방법이 약간 다르다. 자세한건 [여기](https://developer.android.com/preview/api-overview.html#fvbi-signatureView.findViewById() 여기를 참조하십시오.따라서 결과 findViewById)에서 확인할 수 있다.

1. 변경

```
val listView = findViewById(R.id.list) as ListView 에
```

```
val listView = findViewById<ListView>(R.id.list)
```

또 이렇게 하나하나 findviewbyid대신 kotlin extention을 사용하면된다.

```
apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'
apply plugin: 'kotlin-android-extensions' // 익스텐션 플러그인 적용

android {
    ...
}

dependencies {
    ...
}
```

이렇게 모듈 빌드스크립트에 추가해줘야 한다. 


```kotlin 

import android.os.Bundle
import android.support.v7.app.AppCompatActivity
import kotlinx.android.synthetic.main.activity_extension.*

class ExtensionActivity: AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_extension)

        // 버튼 클릭 리스너 설정
        btn_activity_extension.setOnClickListener {

            // 텍스트뷰에 사용자가 입력한 텍스트를 조합한 문자열 표시
            tv_activity_extension_hello.text =
                    "Hello, ${et_activity_extension_name.text.toString()}"
        }

    }
}
```

위와 같이 xml의 이름을 그대로 가져와서 사용할 수 있다.