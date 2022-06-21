### Find Minimum in Rotated Sorted Array


Today's Problem is Medium - [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

### Problem

Suppose an array of length n sorted in ascending order is rotated between 1 and n times. For example, the array nums = [0,1,2,4,5,6,7] might become:

[4,5,6,7,0,1,2] if it was rotated 4 times.
[0,1,2,4,5,6,7] if it was rotated 7 times.
Notice that rotating an array [a[0], a[1], a[2], ..., a[n-1]] 1 time results in the array [a[n-1], a[0], a[1], a[2], ..., a[n-2]].

Given the sorted rotated array nums of unique elements, return the minimum element of this array.

You must write an algorithm that runs in O(log n) time.

 

Example 1:

```
Input: nums = [3,4,5,1,2]
Output: 1
Explanation: The original array was [1,2,3,4,5] rotated 3 times.
```

Example 2:

```
Input: nums = [4,5,6,7,0,1,2]
Output: 0
Explanation: The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.
```

Example 3:

```
Input: nums = [11,13,15,17]
Output: 11
Explanation: The original array was [11,13,15,17] and it was rotated 4 times. 
``` 

Constraints:

```
n == nums.length
1 <= n <= 5000
-5000 <= nums[i] <= 5000
All the integers of nums are unique.
nums is sorted and rotated between 1 and n times.
```

### Solution

```kotlin
class Solution {
    var result: Int = 0
    
    fun findMin(nums: IntArray): Int {
        if (nums[0] < nums[nums.size - 1]) return nums[0]
        find(nums, 0, nums.size - 1)
        return result
    }

    fun find(nums: IntArray, startIndex: Int, endIndex: Int) {
        if (endIndex - startIndex <= 1) {
            result = Math.min(nums[endIndex], nums[startIndex])
            return
        }
        val centerIndex = (endIndex + startIndex) / 2

        if (nums[startIndex] < nums[centerIndex]) {
            find(nums, centerIndex, endIndex)
        } else {
            find(nums, startIndex, centerIndex)
        }
    }
}
```

### Explanation

rotate가 되지 않은 케이스를 제외하면, acending되지 않은 케이스를 찾으면 된다. 반씩 잘라서 centerIndex에 비해 startIndex가 값이 크다면 인덱스 반드시 그 사이에 최소 값이 존재하게 됩니다. sort되어있으므로!