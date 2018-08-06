## DialogFragment안에서 Fragment 사용하기



Fragment 안에 Fragment가 일부분 들어가지 못한다. 코딩을 할 때는 표준을 지켜야 되며, 여기서 표준이란 여기도 돌아가고 저기도 돌아가는 것을 의미한다.

Fragment안에 Fragment는 getChildFragmentManager()메서드를 이용하여야 한다.

아래와 같은 오류가 발생되었을 때

```
IllegalStateException: Fragment does not have a view
```

커스텀 다이얼로그를 만들 때 onreateDialog(..)를 사용하면 DialogFragment는 null View를 가지게 됩니다. (메시지가 그렇게 뜨지요) 일반적으로 다이얼 로그 안에 뷰가 필요하지는 않지요. AlertDialog.builder와 소통하는 건 완벽한 방법이라고 생각되지는 않지만 아래와 같은 방법을 고려할 수 있습니다.

1. onCreateDialog 대신 onCreateView를 오버라이딩하여 사용
2. Fragment로부터 상속 받은 나만의 타입으로 생성하기
3. FragmentPagerAdpater가 아닌 PagerAdapter 사용하기.

부가적으로 dialog 배경 투명하기 만들기

```
  Dialog dialog = new Dialog(getActivity());

    dialog.getWindow().requestFeature(Window.FEATURE_NO_TITLE);

    dialog.getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN, WindowManager.LayoutParams.FLAG_FULLSCREEN);      

        // layout to display
    dialog.setContentView(R.layout.add_edit);

    // set color transpartent
    dialog.getWindow().setBackgroundDrawable(new ColorDrawable(Color.TRANSPARENT));

    dialog.show();
```