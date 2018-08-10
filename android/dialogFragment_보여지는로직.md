## DialogFragment 보여지는 로직

1. 아래와 같이 쇼를 호출한다.
2. 다이얼로그가 생성된다 (Dialog || BottomSheetDialog 등)
3. Fragment의 onCreateView가 호출된다.
4. BottomSheetBehavior 가 생성된다 (BottomSheetDialog에 한해서)
5. 내가 확장한 DialogFragment의 start()가 호출됨
6. 다이얼로그가 노출됨.

```
//show 호출
fragment.show(fragmentManager, RealTimeIssueBottomSheetFragment.TAG);
```