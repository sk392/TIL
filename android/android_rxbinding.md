## Android RxBinding

&nbsp;최근에야 알게된 ~~벌써 버전은 3이지만~~ RxBinding에 대해 조금 정리해본다. [RxBinding](https://github.com/JakeWharton/RxBinding)은 무려 근본이신 Jake Wharton께서 만드셨다. 

&nbsp;RxBinding은 RxJava와 RxAndroid를 이용해 안드로이드의 위젯이나 View에 Rx를 사용하기 쉽게 해주는 오픈소스다. 예를 들면 Button Click, SearchView QueryTextChanger 등 다양한 기능 들이 구현되어 있다.


### Download

&nbsp;아래와 같이 굉장히 다양한 기능들을 보유 하고 있는데 아마 다 알아가려면 시간이 걸릴 것같다.

```
Platform

implementation 'com.jakewharton.rxbinding3:rxbinding:3.1.0'

AndroidX Libraries

implementation 'com.jakewharton.rxbinding3:rxbinding-core:3.1.0'
implementation 'com.jakewharton.rxbinding3:rxbinding-appcompat:3.1.0'
implementation 'com.jakewharton.rxbinding3:rxbinding-drawerlayout:3.1.0'
implementation 'com.jakewharton.rxbinding3:rxbinding-leanback:3.1.0'
implementation 'com.jakewharton.rxbinding3:rxbinding-recyclerview:3.1.0'
implementation 'com.jakewharton.rxbinding3:rxbinding-slidingpanelayout:3.1.0'
implementation 'com.jakewharton.rxbinding3:rxbinding-swiperefreshlayout:3.1.0'
implementation 'com.jakewharton.rxbinding3:rxbinding-viewpager:3.1.0'
implementation 'com.jakewharton.rxbinding3:rxbinding-viewpager2:3.1.0'

Google Material

implementation 'com.jakewharton.rxbinding3:rxbinding-material:3.1.0'
```

### example

&nbsp;간단하게 사용 방법들을 알아보자, 제일 간단하게 클릭을 구현해 보았다.

&nbsp;xml에 간단하게 버튼을 하나 만들어주고 MainActivity에서 해당 버튼에 클릭리스너를 달아봤다.

activity_main.xml

```
    <Button
        android:id="@+id/tv_btn"
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:gravity="center"
        android:text="button"
        android:textSize="20dp" />
```

MainActivity.kt

```
    val view = findViewById<Button>(R.id.tv_btn)
    view.clicks()
        .subscribe {
            Toast.makeText(
                applicationContext,
                "Test 입니당",
                Toast.LENGTH_SHORT
            ).show()
    }.addToDisposable()

```

&nbsp;이렇게하면 기존에 Rx의 형태와 같이 사용할 수 있고, rx에서 지원해주는 기능들은 전부 넣을 수 있으니 쓰기 좋다.

### point

* rx의 기능들 사용 가능

&nbsp;위에서도 설명했듯이 clickListener를 Observable로 대체되어 원하는 rx의 operator를 가져다 쓸 수 있다.

&nbsp;한가지 추가적인 예시를 들자면 화면전환, 결제, 추가 및 삭제요청 등의 작업을 할 때 중복 클릭 방지가 필요한 경우가 있는데, ~~(물론 clickable을 작업에 결과가 오기까지 false를 둔다던가 하는 식으로 처리 해도 된다)~~ 이럴 때 요런식으로 rx Operator의 [debounce](http://reactivex.io/documentation/operators/debounce.html)나 throttleFirst등을 활용해볼 수 있다.

* 기능의 한계

&nbsp;다들 예상하다 싶이 결국엔 Jake Wharton이 지원해주지 않은 기능은 별도로 구현해야한다. 근데 요것은 코드를 보다보면 어느 정도 컨벤션이 있는 것을 확인할 수 있고, 별도의 Extention들을 뽑아 놓는 클래스를 둬도 되지 않을까 싶다. 

* 러닝 커브

&nbsp;기능에 대한 명세서가... 딱히 없고..? ~~(못찾는거면 풀리퀘좀 날려주세여)~~
대부분 View에대한 처리들은(click, attach, detach 등) 찾기 쉽지만 다른 것들은 제공해주는지 먼저 봐가면서 학습하는 시간이 필요할 것같다.

### MainThread?

&nbsp;요것을 보면서 가장 궁금했던거는 Rx를 사용해봤던 사람이라면 알겠지만 뷰에 대한 작업을 하려면 작업하는 쓰레드가 AndroidSchedulers.mainThread()에서 해야되는데 요것을 어떻게 처리 했을까 굉장히 궁금했다.

&nbsp;제이크 왓슨님이 어떤식으로 구현했는지 보자.

&nbsp;가장 심플한 [View.clicks()](https://github.com/JakeWharton/RxBinding/blob/master/rxbinding/src/main/java/com/jakewharton/rxbinding3/view/ViewClickObservable.kt)에 대한 구현을 보자

```
@CheckResult
fun View.clicks(): Observable<Unit> {
  return ViewClickObservable(this)
}

private class ViewClickObservable(
  private val view: View
) : Observable<Unit>() {

  override fun subscribeActual(observer: Observer<in Unit>) {
    if (!checkMainThread(observer)) {
      return
    }
    val listener = Listener(view, observer)
    observer.onSubscribe(listener)
    view.setOnClickListener(listener)
  }

  private class Listener(
    private val view: View,
    private val observer: Observer<in Unit>
  ) : MainThreadDisposable(), OnClickListener {

    override fun onClick(v: View) {
      if (!isDisposed) {
        observer.onNext(Unit)
      }
    }

    override fun onDispose() {
      view.setOnClickListener(null)
    }
  }
}
```

&nbsp;요런식으로 구현되어 있고 Observable을 구현할 때 반드시 구현해야하는 subscribeActual에서 mainThread인지 체크한 후에 반환하고 별도의 ThreadChange는 구현되어 있지 않은 것을 볼 수 있습니다. 뭔가 예상하기론 MainThread로 전환해주는 코드가 있지 않을까 했는데 아니었습니다.

&nbsp;어차피 밑에 Listener를 보면 onClickListener에서 onClick이 발생하면 observer.onNext(Unit)을 해주기 때문에 들어오는 Thread는 이미 MainThread이기 때문이죠!


### MainThreadDisposable

&nbsp;이걸 보다보니 또 궁금한게 Listener가 구현하고 있는 MainThreadDisposable이라는 것도 있는데 요게 뭔지 궁금하네요.

```
public abstract class MainThreadDisposable implements Disposable {

    public static void verifyMainThread() {
        if (Looper.myLooper() != Looper.getMainLooper()) {
            throw new IllegalStateException(
                "Expected to be called on the main thread but was " + Thread.currentThread().getName());
        }
    }

    private final AtomicBoolean unsubscribed = new AtomicBoolean();

    @Override
    public final boolean isDisposed() {
        return unsubscribed.get();
    }

    @Override
    public final void dispose() {
        if (unsubscribed.compareAndSet(false, true)) {
            if (Looper.myLooper() == Looper.getMainLooper()) {
                onDispose();
            } else {
                AndroidSchedulers.mainThread().scheduleDirect(new Runnable() {
                    @Override public void run() {
                        onDispose();
                    }
                });
            }
        }
    }

    protected abstract void onDispose();
}

```

&nbsp;코드를 보면 심플합니다. subscribe여부를 atomic으로 Threadsafety하게 처리되어 있고 dispose에 대한 처리를 MainThread에서 처리해주도록 하는 것을 볼 수 있습니다. 뭔가.. 이 Disposable이 필요한 경우가 얼마나 있을진 잘 모르겠지만, 참고할만 하다고 생각된다!

&nbsp; P.S - 뭔가 간단하게 MainThread에서 작업할 일이 있으면 그냥 Handler를 통해서 자주 작업했는데 dispose쪽 코드를 보면 

```
AndroidSchedulers.mainThread().scheduleDirect(new Runnable())
```

요렇게도 처리할 수 있는 것을 처음 알았다. 좀 더 rx적인 코드를 짤 수 있게 된 것같다.