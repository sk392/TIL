### Find First and Last Position of Element in Sorted Array



오늘의 문제는 Medium-  [Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/all-possible-full-binary-trees/)

### Problem

Given an array of integers nums sorted in ascending order, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order of O(log n).

If the target is not found in the array, return [-1, -1].

Example 1:

```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

Example 2:

```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

### Solution

```
class Solution {
    fun searchRange(nums: IntArray, target: Int): IntArray {
        var l = 0
        var r = nums.size - 1
        var targetIndex = -1

        loop@ while (l <= r) {
            val mid = (l + r) / 2
            val value = nums[mid]
            when {
                value == target -> {
                    targetIndex = mid
                    break@loop
                }
                value > target -> {
                    r = mid - 1
                }
                value < target -> {
                    l = mid + 1
                }
            }
        }
        if (targetIndex != -1) {
            l = targetIndex
            r = targetIndex
            while (l >= 0 && nums[l] == target) {
                l--
            }
            while (r < nums.size && nums[r] == target) {
                r++
            }
            return intArrayOf(l + 1, r - 1)
        } else {
            return intArrayOf(-1, -1)
        }
    }
}
```

### Explanation

아.. 일단 느린 방법으로라도 확실한 방법으로 먼저 풀어보자!!!! 또 뇌버깅하고 더 효율적으로 짜려다가 시간만 버렸다. 이걸로도 상위 90퍼센트는 나오네.. 상황에 따라 최악에 O(n)까지 나올텐데 그래도 완벽해결하는 방향으로!