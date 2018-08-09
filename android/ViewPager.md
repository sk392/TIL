## Android ViewPager

안드로이드의 뷰 페이저는 실제로 여러 화면을 그리고 있는게 아니라, 하나의 긴 화면을 유지하고 있는 것이다. 

가로 사이즈를 계산한 다음 offScreenPageLimit의값 x 가로사이즈 만큼의 뷰를 그린다.

스크롤해서 이동할 때는 한번에 가로사이즈 만큼을 움직이게 되는데, 이 부분은 ViewPager의 ScrollToItem을 보면 destX가 가로사이즈만큼 움직인 다는 것을 알 수 있다.



### 로직순서

1. populate(int)에서 아이템을 추가하게 되는데 추가하는 메소드는 addNewItem()이다.
	1. 처음에 첫 포지션의 아이템 (position : 0)을 추가한 뒤
	2. 현재 위치보다 왼쪽의 아이템을 추가하고
	3. 현재 위치보다 오른쪽의 아이템을 추가한다.
	4. 왼쪽, 오른쪽의 아이템을 추가할 때 각각 leftWidthNeeded,RightWidthNeeded라는 기준점을 활용하는데, 이는 패딩값이 없다면 정수로, 있다면 정수로 떨어지지 않는다. (패딩 값이 있다면 값은 1.xxxx)
	5. 저 값이 패딩이 존재할 경우 정수 값보다 커지게 되므로 pageLimit의 값보다 하나씩 더 가져올 수 있다.
2. 스크롤 이동.
	1. 스크롤 시ScrollToItem이라는 메소드에서 스크롤을 관리하게 된다.
	2. 스크롤은 가로로 긴 레이아웃이 있다고 가정하고, 가로 사이즈만큼의 길이를 오른쪽 혹은 왼쪽으로 이동하게 되는 것이다.
	3. 움직일 위치는 destX라는 값을 보면 확인할 수 있는데, 이 값은 패딩을 제외한 viewPager의 온전한 사이즈를 기준으로 정수배한 것을 알 수 있다.
	4. 스크롤로(터치) 이동 시에는 smoothScroll이 true이고 아닌 경우(탭레이아웃 등으로 인한 프로그래밍적인 움직임) false이다.
3. 아이템 추가 및 삭제되는 순서
	1. 왼쪽 스크롤 시에
    	1. 왼쪽 아이템 추가 (FragmentTransaction에 Fragment add)
    	2. 오른쪽 아이템 삭제 (FragmentTransaction에 Fragment Dettatch)
	2. 오른쪽 스크롤 시에
   		1. 왼쪽 아이템 삭제(FragmentTransaction에 Fragment Dettatch)
   		2. 오른쪽 아이템 추가(FragmentTransaction에 Fragment add)


   		
### 다음앱의 AppViewPager & FragmentPagerAdapter의 문제

1. 프리로드 되는 아이템의 개수가 보여지는 Tab의 개수보다 많은 경우
2. 기존에 있던 아이템을 삭제 및 Fragment Dettatct-> 새로운 아이템을 추가 (FragmentTransaction에 Fragment add)를 해야하는데
3. 이 때 Fragment는 TAG기반으로 운영되므로, 해당 태그가 존재한다면 새로 만들지 않고, 기존의 애를 Attatch해준다.
4. 기존의 Fragment는 해당 좌표가 옛날 좌표임에 비해 보여주는 페이지는 새로운 좌표(오른쪽 혹은 왼쪽 끝부문)이므로 화면에 노출되지 않는다
5. 단 아이템의 보여짐이나 정보는 정상적으로 나오게 된다.
 

