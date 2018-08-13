## ItemDecoration


#### getItemOffsets(Rect outRect, View view, RecyclerView parent, RecyclerView.State state)

이 곳에서는 아이템 외각 라인의 Rect을 준다. 외각 라인은 아이템 영역에 속하지 않고 일종의 마진 역할을 하게 되는데, 단 주의해야 하는 점은, 아이템을 클릭했을 때 아이템 영역에 리플 이펙트가 발생하게 된다.

이 때 위에서 설명했듯이 outRect은 아이템 영역이 아니므로 리플 효과가 없기 때문에 주의해서 사용해야 한다.


#### onDrawOver(Canvas c, RecyclerView parent, RecyclerView.State state)

이 곳은 아이템을 기준으로 Draw를 하게 되는데, getItemOffsets()와는 다르게 실제로 그림을 그리게 된다. 

이 때 메소드 이름에서도 알 수 있듯이 아이템의 위 부분에 그리게 되는데, 이는 아이템 효과를 받지 못한다는 의미이기도 하다.

아이템 추가 삭제 시 애니메이션을 넣는 경우에 여기서 그려진 DrawOver는 애니메이션과 함께 움직이지 않는 점을 알 수 있다.

그런 경우는 onDraw에다가 그려주는 것이 좋다.

