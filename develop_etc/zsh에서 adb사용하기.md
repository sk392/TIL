## ZSH에서 ADB 사용하기

adb를 사용하다가 zsh로 터미널을 대체한 후에 adb가 안되서 확인해보닝 bash_profile에 해당 path가 없어서 그렇다고 한당.

다음과 같이 해당 bash_profile에 path를 작성해주고

```
echo 'export ANDROID_HOME=/Users/$USER/Library/Android/sdk' >> ~/.bash_profile

echo 'export PATH=${PATH}:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools' >> ~/.bash_profile
```

내가 가지고 있던 bash_profile을 아래와 같이 리프레시 해주고

```
source ~/.bash_profile
```

사용하고 있던 AndroidStudio를 재실행 해주면 adb를 사용할 수 있다. 