# IncompatibleClassChangeError


 Gson에서 IncompatibleClassChangeError라는 익셉션이 떨어졌고, 메시지가 ```Couldn't find com.google.gson.annotations.SerializedName.value```라고 프린되는데, 요건 Reflection을 사용해서 annotation을 처리하는 와중에 "value"라는 이름의 Method를 찾을 수없어서 발생하는 크래시가 발생했었다.
 
## 발생환경
 
 OS = 5.0 / 5.0.1

Device = SM-N900L(갤럭시 노트3), SM-N900S(갤럭시 노트3), SHV-E330K(갤럭시S4)

## 재현 경로

???

## StackTrace

Exception = IncompatibleClassChangeError

Message = Couldn't find com.google.gson.annotations.SerializedName.value


```java
at  libcore.reflect.AnnotationAccess.toAnnotationInstance(AnnotationAccess.java:659)
at  libcore.reflect.AnnotationAccess.toAnnotationInstance(AnnotationAccess.java:641)
at  libcore.reflect.AnnotationAccess.getDeclaredAnnotation(AnnotationAccess.java:170)
at  java.lang.reflect.Field.getAnnotation(Field.java:242)
at  com.google.gson.internal.bind.ReflectiveTypeAdapterFactory.getFieldNames(ReflectiveTypeAdapterFactory.java:74)
at  com.google.gson.internal.bind.ReflectiveTypeAdapterFactory.getBoundFields(ReflectiveTypeAdapterFactory.java:161)
at  com.google.gson.internal.bind.ReflectiveTypeAdapterFactory.create(ReflectiveTypeAdapterFactory.java:102)
at  com.google.gson.Gson.getAdapter(Gson.java:458)
at  com.google.gson.internal.bind.ReflectiveTypeAdapterFactory.createBoundField(ReflectiveTypeAdapterFactory.java:117)
at  com.google.gson.internal.bind.ReflectiveTypeAdapterFactory.getBoundFields(ReflectiveTypeAdapterFactory.java:166)
at  com.google.gson.internal.bind.ReflectiveTypeAdapterFactory.create(ReflectiveTypeAdapterFactory.java:102)
at  com.google.gson.Gson.getAdapter(Gson.java:458)
at  com.google.gson.internal.bind.ReflectiveTypeAdapterFactory.createBoundField(ReflectiveTypeAdapterFactory.java:117)
at  com.google.gson.internal.bind.ReflectiveTypeAdapterFactory.getBoundFields(ReflectiveTypeAdapterFactory.java:166)
at  com.google.gson.internal.bind.ReflectiveTypeAdapterFactory.create(ReflectiveTypeAdapterFactory.java:102)
at  com.google.gson.Gson.getAdapter(Gson.java:458)
at  com.google.gson.internal.bind.ReflectiveTypeAdapterFactory.createBoundField(ReflectiveTypeAdapterFactory.java:117)
at  com.google.gson.internal.bind.ReflectiveTypeAdapterFactory.getBoundFields(ReflectiveTypeAdapterFactory.java:166)
at  com.google.gson.internal.bind.ReflectiveTypeAdapterFactory.create(ReflectiveTypeAdapterFactory.java:102)
at  com.google.gson.Gson.getAdapter(Gson.java:458)
at  retrofit2.converter.gson.GsonConverterFactory.responseBodyConverter(GsonConverterFactory.java:64)
at  retrofit2.Retrofit.nextResponseBodyConverter(Retrofit.java:330)
at  retrofit2.Retrofit.responseBodyConverter(Retrofit.java:313)
at  retrofit2.HttpServiceMethod.createResponseConverter(HttpServiceMethod.java:113)
at  retrofit2.HttpServiceMethod.parseAnnotations(HttpServiceMethod.java:82)
at  retrofit2.ServiceMethod.parseAnnotations(ServiceMethod.java:37)
at  retrofit2.Retrofit.loadServiceMethod(Retrofit.java:170)
at  retrofit2.Retrofit$1.invoke(Retrofit.java:149)
at  java.lang.reflect.Proxy.invoke(Proxy.java:397)
...
```

## 상세


Gson에도 [관련된 이슈](https://github.com/google/gson/issues/726#issuecomment-159010233)가 등록되었으나 Annotation 이름을 Value로 한게 이유 처럼 보임.


[AnnotationAccess.java](https://android.googlesource.com/platform/libcore/+/kitkat-release/luni/src/main/java/libcore/reflect/AnnotationAccess.java#689)

해당 라인에서 찍힌 에러 메시지가 "Couldn't find com.google.gson.annotations.SerializedName.value"이고
```java
            try {
                method = annotationClass.getMethod(nameString, NO_ARGUMENTS);
            } catch (NoSuchMethodException e) {
                throw new IncompatibleClassChangeError(
                        "Couldn't find " + annotationClass.getName() + "." + nameString);
            }
```

class.java에서 getMethod로 찾을 때 못찾아서 에러가 발생했는데.. 에러가 발생한 지점의 class.java를 찾기는 쉽지 않아보인다..

```java
    public Method getMethod(String name, Class<?>... parameterTypes)
        throws NoSuchMethodException, SecurityException {
        return getMethod(name, parameterTypes, true);
    }
    ```
    
하지만 위에 에러메시지가의 nameString이 "value"인 것을 보아(다른 크래시 레포트도 동일) SerializedName 이름이 "value"라서 나는 이슈일 것같다는 추론을 해볼 수는 있을 것같다.

명확하게 대처하기 어렵고 재현도 쉽지 않아 일단 SerializedName 이름을 제거해보고 나가는건 어떨까 생각해봤지만 크래시 이유가 reflection에서 발생하는 거라 reflection을 사용하지 않고 code gen을 활용하는 moshi를 적용하므로써 해결할 예정