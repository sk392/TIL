## ViewPager에서 WrapContent가 동작하지 않을 때


뷰페이저로 작업하다보면, wrapContent가 작동하지 않을 때가 있다. wrapContent는 하위뷰의 사이즈를 알아야하는데, 이를 알 수 없기 때문에 wrapContent는 정상작동하지 않고, match_parent로 작동하는 것을 알 수 있다.

이를 회피하는 방법은 2가지인데, 

1. xml에서 height를 dp로 직접 선언한다
2. ViewPager가 뷰를 그릴 때[Measure()함수] Child 뷰를 모두 그리면서 가장 높은 값을 찾고, 그 사이즈를 딱 맞춰서 그리게 한다.

2번은 아래와 같은 코드를 사용하면 된다.


```

public class WrapContentViewPager extends ViewPager {
        public WrapContentViewPager(Context context) {
            super(context);
        }

        public WrapContentViewPager(Context context, AttributeSet attrs) {
            super(context, attrs);
        }

        @Override
        protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {

            int height = 0;
            for (int i = 0; i < getChildCount(); i++) {
                View child = getChildAt(i);
                child.measure(widthMeasureSpec, MeasureSpec.makeMeasureSpec(0, MeasureSpec.UNSPECIFIED));
                int h = child.getMeasuredHeight();
                if (h > height) height = h;
            }

            heightMeasureSpec = MeasureSpec.makeMeasureSpec(height, MeasureSpec.EXACTLY);

            super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        }
    }
```