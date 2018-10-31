## Async ViewStub

안드로이드에서 레이아웃을 그릴 때 계층이 많거나, 많은 요소를 그려야할 경우 (e.g. ScrollView와 같은 경우엔 노출 되는 모든 레이아웃을 한번에 그려야 한다, 디바이스 화면의 몇 배가 되던간) 부하가 급격하게 많이 걸리고, 이는 사용자가 볼 때 버벅거린다고 생각하고 당신의 서비스에 별점을 1점을 줄지 모른다. (흑흑) 그럴 때 사용하는 것이, ViewStub인데, 우리는 ViewStub을 통해 레이아웃의 인플레이션을 분리할 수 있다! 단계적으로 레이아웃을 인플레이션 시킴으로써 사용자는 느리지 않다고 생각할 것이다. 이 때 ViewStub이란 다음과 같다. 

### ViewStub

안드로이드에서 [ViewStub](https://developer.android.com/reference/android/view/ViewStub) 은 해당 레이아웃을 인플레이트를 원하는 시점에 할 수 있도록 해주는 커스텀 레이아웃이다. 

보여야 하는 레이아웃이 아주 무거워 단계적으로 인플레이트를 한다던가, 자주 사용하지는 않는데 해당 레이아웃이 무겁고 반드시 모든 사용자에게 그려질 필요가 없는 경우에 사용한다. 자주 사용하지는 않는다.


### aync ViewStub

하지만 처음 설명과 같이 ViewStub을 쓰더라도 문제점이 남아있다. 바로 ViewStub의 인플레이트는 MainThread에서 발생하므로, ViewStub자체가 무겁게되면 우리가 우려했던 부분이 다시 재조명 된다. 그럴 때 우린 비동기 뷰스텁으로 문제를 해결할 수 있다!

비동기 스텁은 ViewStub 및 AsyncLayoutInflater를 사용해서 만들어지게 된다. 기존의 inflate()랑은 다른 방법으로!

```@kotlin
class AsyncViewStub @JvmOverloads constructor(
        context: Context, set: AttributeSet? = null, defStyleAttr: Int = 0
) : View(context, set, defStyleAttr) {
    private val inflater: AsyncLayoutInflater by lazy { AsyncLayoutInflater(context) }

    private var inflatedId = NO_ID
        get
    private var layoutRes = 0

    init {
        val attrs = intArrayOf(android.R.attr.layout, android.R.attr.inflatedId)
        context.obtainStyledAttributes(set, attrs, defStyleAttr, 0).use {
            layoutRes = getResourceId(0, 0)
            inflatedId = getResourceId(1, NO_ID)
        }

        visibility = View.GONE
        setWillNotDraw(true)
    }

    override fun onAttachedToWindow() {
        super.onAttachedToWindow()
        if (isInEditMode) {
            val view = inflate(context, layoutRes, null)
            (parent as? ViewGroup)?.let {
                it.addViewSmart(view, it.indexOfChild(this), layoutParams)
                it.removeViewInLayout(this)
            }
        }
    }

    override fun onMeasure(widthMeasureSpec: Int, heightMeasureSpec: Int) {
        setMeasuredDimension(0, 0)
    }

    override fun draw(canvas: Canvas) {}

    override fun dispatchDraw(canvas: Canvas) {}

    fun inflate(listener: AsyncLayoutInflater.OnInflateFinishedListener? = null) {
        val viewParent = parent

        if (viewParent != null && viewParent is ViewGroup) {
            if (layoutRes != 0) {
                inflater.inflate(layoutRes, viewParent) { view, resId, parent ->
                    if (inflatedId != NO_ID) {
                        view.id = inflatedId
                    }

                    val stub = this
                    val index = parent.removeViewInLayoutIndexed(stub)
                    parent.addViewSmart(view, index, layoutParams)
                    listener?.onInflateFinished(view, resId, parent)
                }
            } else {
                throw IllegalArgumentException("AsyncViewStub must have a valid layoutResource")
            }
        } else {
            throw IllegalStateException("AsyncViewStub must have a non-null ViewGroup viewParent")
        }
    }

    private fun ViewGroup.removeViewInLayoutIndexed(view: View): Int {
        val index = indexOfChild(view)
        removeViewInLayout(view)
        return index
    }

    private fun ViewGroup.addViewSmart(child: View, index: Int, params: LayoutParams? = null) {
        if (params == null) addView(child, index)
        else addView(child, index, params)
    }
}

```

우리가 자세하게 봐야하는 부분은 바로 inflate()이다. 

사용 방법은 다음과 같다. xml에서 아래와 같이 추가해주고, 기존의 ViewStub이랑 비슷하게 layout을 지정해줘야 한다.

```
<widgets.AsyncViewStub
    android:id="@+id/avs_stub"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:inflatedId="@+id/awesome_view"
    android:layout="@layout/awesome_layout"/>
```

그 후 아래와 같이 해당 callback을 달아서 호출하면 된다! 멋져!

```
private inline fun <T : View> prepareStubView(
        stub: AsyncViewStub, 
        crossinline prepareBlock: T.() -> Unit
) {
    val inflatedView = getBinding().root.findViewById(stub.inflatedId) as? T
    if (inflatedView != null) {
        inflatedView.prepareBlock()
    } else {
        stub.inflate(AsyncLayoutInflater.OnInflateFinishedListener { view, _, _ ->
            (view as? T)?.prepareBlock()
        })
    }
}
```

원문을 재정리 한 것이나.. 넘나 맘에 든다! [멋져!](https://medium.com/@programmerr47/async-view-stubs-394e9f2d79ec)