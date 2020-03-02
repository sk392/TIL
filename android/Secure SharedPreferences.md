## Secure SharedPreferences

 기존에 손 쉽게 데이터를 저장할 수 있게 도와주는 SharedPreference를 사용할 때 데이터를 안전하게 저장하는 방법을 알아보겠읍니다.

### SharedPreference

 안드로이드 공식 사이트의 저장소 개발 가이드 문서는 데이터를 저장하는 여러 가지 방법을 소개하고 있습니다. 그 중 ‘내부 저장소’ 의 다음 특징은 눈여겨볼 만 합니다.

> 기기의 내부 저장소에 파일을 직접 저장할 수 있습니다. 기본적으로, 내부 저장소에 저장된 파일은 해당 애플리케이션의 전용 파일이며 다른 애플리케이션(및 사용자)은 해당 파일에 액세스할 수 없습니다. 사용자가 애플리케이션을 제거하면 해당 캐시 파일은 제거됩니다.

 즉, 다른 애플리케이션에 노출하면 곤란한 중요한 정보들은 내부 저장소에 담아두면 안전하다고 할 수 있습니다. 하지만, 정말일까요?

 앱의 데이터는 ```/data/data/com.securecompany.secureapp``` 에 저장되어 있습니다만, 앱을 release 모드로 빌드하면 adb 명령으로도 볼 수 없으니 안전하다고 할 수 있을 겁니다. 하지만 안드로이드는 루팅이 굉장히 쉽기 때문에 release 모드로 빌드하더라도 adb 명령에 접근이 가능해서 내부 데이터를 확인 할 수 있게 됩니다.

### 암호화를 한다면

 어차피 보여줄 거라면 암호화하면 될 것같습니다. 하지만 암호화 할 때 중요한 것이 있습니다. 안드로이드는 디컴파일이 매우 쉬운 플랫폼이기 때문에 우리 로직 내에 String형태로 키를 관리하는 방식은 완벽하게 암호화되어 있다고 말 할 수 없습니다.
 여기서 일부 사람들은 JNI를 떠올릴지도 모르겠습니다만, JNI로 컴파일한 .so파일도 objdump 같은명령어로 내용을 다 볼 수 있기 떄문에 안전하다고 볼 수 없습니다. 또한 kotlin 구현처럼 ``` static const``` 형태로 소스코드에 적어두면 공격자 입장에서는 [.data세그먼트](!https://en.wikipedia.org/wiki/Data_segment)만 확인하면 되니 큰일입니다. 여기서 .data 세그먼트를 회피하기 위해서 로직을 키로 생성하여 작성하더라도 숙련된 공격자라면 .text 세그먼트를 찾아서 실마릴 찾을 수 이쓸 겁니다.
 
### 어쩌라는거야
 
[Android KeyStore 시스템](!https://developer.android.com/training/articles/keystore) 문서 첫 머리에 적혀있는 글은 다음과 같습니다.


> The Android Keystore system lets you store cryptographic keys in a container to make it more difficult to extract from the device. Once keys are in the keystore, they can be used for cryptographic operations with the key material remaining non-exportable. Moreover, it offers facilities to restrict when and how keys can be used, such as requiring user authentication for key use or restricting keys to be used only in certain cryptographic modes.

> Android Keystore 시스템은 암호화 키를 ‘컨테이너’ 에 저장하도록 해 기기에서 키를 추출하기 더욱 어렵게 해 줍니다. 일단 키를 Keystore 에 저장하면 키를 추출 불가능한 상태로 암호화에 사용할 수 있습니다. 또한 Keystore 는 키 사용 시기와 방법(예: 사용자 인증 등의 상황)을 통제하고, 특정 암호화에서만 키를 사용하도록 허용하는 기능도 제공합니다.

 
 좀더 쉽게 다시 설명하자면, 암호화에 쓸 키를 소스코드 내부 어딘가가 아니라, 시스템만이 접근 가능한 어딘가(컨테이너)에 저장해 문제를 해결해 준다는 뜻입니다. 여기서 키가 저장되는 ‘컨테이너’ 는 기기별로 구현이 다를 수 있습니다만 핵심은 사용자 어플리케이션이 그 영역에 접근할 수 없다는 점입니다. 이 때문에 KeyStore 를 사용해서 키를 안전하게 저장할 수 있습니다.
 
 
### Secure SharedPreferences 구현하기

 앞서 설명드렸던 KeyStore 를 사용해 SharedPreferences 의 내용을 암호화하는 로직입니다. 소스 코드의 길이가 꽤 길기에, github gist 링크로 대신합니다. 

 [AndroidCipherHelper.kt](!https://gist.github.com/FrancescoJo/b8280cff14f1254f2185a9c2e927565e) - KeyStore 에서 생성한 랜덤 패스워드를 이용해 입력받은 문자열을 암호화 하는 로직. IV 설정 등 귀찮은 작업을 피하기 위해 비대칭 암호화 알고리즘을 사용했다. 또한 암호화 및 복호화 과정에서 비대칭키의 Public key 로 암호화하고, Private key 로 해독하도록 구현했다. TEE 를 올바르게 구현한 기기(안드로이드 23 이상 + 메이저 하드웨어 제조사)에서 동작하는 한, 이 데이터의 내용이 유출되더라도 복호화는 오직 이 로직 내부에서만 할 수 있다.

[SecureSharedPreferences.kt](!https://gist.github.com/FrancescoJo/8753a63e1c6888c5d07ceb552c98104c) - AndroidCipherHelper 가 문자열 위주로 암호화하므로, 모든 입력값을 문자 형태로 변환 후 입출력한다.


### 그럼 이제 안전한거야?

다시 Android KeyStore 시스템의 설명으로 돌아가 봅시다.

>Key material of Android Keystore keys is protected from extraction using two security measures:
>…
>Key material may be bound to the secure hardware (e.g., Trusted Execution Environment (TEE), Secure Element (SE)) of the Android device. When this feature is enabled for a key, its key material is never exposed outside of secure hardware.


>Android KeyStore 는 키의 추출을 방지하기 위해 두 가지 보안 조치를 사용합니다.
>…
>키는 안드로이드 기기의 보안 하드웨어(e.g., Trusted Execution Environment (TEE), Secure Element (SE)) 에서만 동작할 수 있습니다. 이 기능이 활성화되면 키는 절대로 보안 하드웨어 밖으로 노출되지 않습니다.

여기서 중요한 부분은 Key material **may be** bound to the secure hardware인데, is가 아니라 may be라는 겁니다. 즉 키가 하드웨어에 저장되지 않을 수도 있다는 사실인데, 문서에 언급되어 있지는 않지만 안드로이드 시스템 특징상 제조원가 절감을 위해 디바이스 제조사들이 KeyStore를 소프트웨어로 구현할 수 도 있습니다. 
 즉, (Android KeyStore)를 쓰더라도 Android M 이전의 기기에서는 우리 앱의 데이터가 100% 안전하다는 장담을 할 수는 없습니다. 아직까지 이 문제를 해결할 방법은 찾지 못했습니다만 아래와 같은 로직으로 ‘이 기기에서의 앱 실행은 안전하지 않을 수 있다’ 같은 안내를 띄우는 정도의 가이드는 개발 가능합니다.

[출처 : 하이퍼커넥트 기술블로그](!https://hyperconnect.github.io/2018/06/03/android-secure-sharedpref-howto.html)