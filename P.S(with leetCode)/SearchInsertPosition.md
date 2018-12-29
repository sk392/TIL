### SearchInsertPosition


요번엔 [SearchInsertPosition](https://leetcode.com/problems/search-insert-position/)요거를 풀어봐씁니다.

### Problem

Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

Example 1:

```
Input: [1,3,5,6], 5
Output: 2
```
Example 2:

```
Input: [1,3,5,6], 2
Output: 1
```
Example 3:

```
Input: [1,3,5,6], 7
Output: 4
```
Example 4:

```
Input: [1,3,5,6], 0
Output: 0
```

### Solution

```
class Solution {
    fun searchInsert(nums: IntArray, target: Int): Int {
        var result =-1
        var tempArray = nums
        var left = 0
        var right = nums.size
        while(result == -1){
            if(left == right) result = left
            val position = left + tempArray.size /2
            when {
                nums[position]==target -> result = position
                nums[position]> target ->{
                    if(position == 0){
                        result = 0
                    }
                    if(tempArray.size == 1){
                        result = left
                    }
                    right = position
                    tempArray = nums.copyOfRange(left,right)
                }
                else -> {
                    if(position == nums.size-1){
                        result = nums.size
                    }
                    if(tempArray.size == 1){
                        result = right
                    }
                    left = position +1
                    tempArray = nums.copyOfRange(left,right)

                }
            }
        }

        return result
    }
}
```

### Explanation

엄.... 적절한 위치를 찾는 것인데, left와 right 포지션으로 서브스트링을 만들고 그 안에서 또 비교하는 식으로 진행했다. (그냥 포지션값만 계산하려니 예외 상황이 자꾸 만들어지더라..ㅠㅠ)

요지는 원본 리스트에서 비교할만한 포지션 값을 찾고 (왼쪽 기준점으로 부터 서브스트링의 절반을 더해서) 타겟이 더 크면 right를 현재 포지션으로, 반대면 left를 현재 포지션 +1로 해준다. 

그러다 값이 끝값이면 각 끝값을 넣어주고, left와 right의 값이 같아지면 해당 값이 포지션이 된다!ㄴ



##### right는 현재 포지션이고 left는 +1인 이유

나는 kotlin의 copyofRange를 썼는데 해당 메서드의 주석을 참고!

```
Returns a new array which is a copy of the specified range of the original array.

@param fromIndex the start of the range (inclusive), must be in `0..array.size`
@param toIndex the end of the range (exclusive), must be in `fromIndex..array.size`

```