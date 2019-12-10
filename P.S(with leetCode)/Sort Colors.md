### Sort Colors


Today's Problem Medium -  [Sort Colors](https://leetcode.com/problems/sort-colors/)

### Problem

Given an array with n objects colored red, white or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

Note: You are not suppose to use the library's sort function for this problem.

Example:

```
Input: [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```

Follow up:

* A rather straight forward solution is a two-pass algorithm using counting sort.
First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.
* Could you come up with a one-pass algorithm using only constant space?


### Solution

```
class Solution {
    fun sortColors(nums: IntArray): Unit {
        var lastZeroIndex = -1
        var lastOneIndex = -1

        for (i in 0 until nums.size) {
            val current = nums[i]

            when (current) {
                0 -> {
                    if (i - 1 >= 0 && nums[i - 1] == 2) nums[i] = 2

                    if (lastOneIndex != -1 && lastOneIndex < i) {
                        lastOneIndex++
                        nums[lastOneIndex] = 1
                    }

                    lastZeroIndex++
                    nums[lastZeroIndex] = 0
                }
                1 -> {
                    if (i - 1 >= 0 && nums[i - 1] == 2) nums[i] = 2

                    if (lastOneIndex == -1) {
                        lastOneIndex = lastZeroIndex
                    }
                    lastOneIndex++
                    nums[lastOneIndex] = 1
                }
            }
        }
    }
}
```

### Explanation

소트만 하면 해결할 수 있는 문제인데 Follow up을 보면 몇개의 공간과 한번의 패스만으로 해결 할 수 있는지가 키 포인트다. 해당 정렬은 몇개의 숫자(0,1,2)만 사용하므로 각 숫자의 마지막 위치를 저장해 둔다면 쉽게 해결할 수 있다.

0, 1, 2, 2, 0의 상황이면 lastOneIndex = 1이고, lastZeroIndex = 0이다. 그리고 이전 값이 2이므로(원래는 lastTwoIndex를 사용했다), lastZeroIndex++, lastOneIndex++, 해주고각각 늘어난 자리에 0과 1을 넣어준 후에 마지막 0자리엔 2를 세팅하면된다.