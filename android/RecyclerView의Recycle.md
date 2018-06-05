## RecyclerView의 재사용 시나리오
1. 스크롤을 아래로 내리면
2.  LinearLayoutManager의 Scro.lVerticallBy 메소드가 호출되고 메소드를 타고타고 가다보면
3.  화면에서 스크롤 할 때 위로 사라지는 viewHolder가 Scrapped View로 지정되는 것이다. (LinearLayoutManager를 통해서)
4. Recycler의 getViewForPosition 메소드가 호출되고
5. LayoutManager로부터 배치하고자 하는 view의 position을 인자로 받아서 adapter의 getItemCount를 통해 유효한 position인지 체크하고
6. getItemViewType을 호출해 현재 position의 viewType을 알아낸다.
7. 그리고 onCreateViewHoldere를 호출 하기 전에 Scrapped view pool에 현재 viewType과 같은 ViewHolder가 있는지 확인하고
8. 있다면 viewHolder를 반환하고 Create하지 않는다.
9. 없다면 onCreateViewHolder에서 viewType에 해당하는 ViewHolde를 생성해서 return
10. onBindViewHolder에서 생성된 viewHolder와 position을 전달받아서 현재 position에 맞는 data를 view들에게 binding해준다.

 
![alt text](http://postfiles8.naver.net/20160413_295/mail1001_1460535517642jTwnI_PNG/3.png?type=w1)

###scrapped view?
scrapped view란, 아직 RecyclerView에는 붙어있지만, 곧 제거되거나 재사용될 것으로 지정된 viewHolder이다.
