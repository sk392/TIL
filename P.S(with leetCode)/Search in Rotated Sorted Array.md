### Search in Rotated Sorted Array



오늘의 문제는 Medium [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

### Problem

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., [0,1,2,4,5,6,7] might become [4,5,6,7,0,1,2]).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

Your algorithm's runtime complexity must be in the order of O(log n).

Example 1:

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

Example 2:

```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

### Solution

```
class Solution {
    fun search(nums: IntArray, target: Int): Int {
        var leftIndex = 0
        var rightIndex = nums.size - 1
        var midIndex: Int


        while (leftIndex <= rightIndex) {
            midIndex = (leftIndex + rightIndex) / 2
            when {
                nums[midIndex] == target -> {
                    return midIndex
                }
                nums[midIndex] > nums[rightIndex] && (target < nums[leftIndex] || target > nums[midIndex]) -> {
                    leftIndex = midIndex + 1
                }
                nums[midIndex] > nums[rightIndex] -> {
                    rightIndex = midIndex - 1
                }
                nums[midIndex] <= nums[rightIndex] && (target > nums[rightIndex] || target < nums[midIndex]) -> {
                    rightIndex = midIndex - 1
                }
                nums[midIndex] <= nums[rightIndex] -> {
                    leftIndex = midIndex + 1
                }
            }
        }

        return -1
    }
}
```

### Explanation

맨처음 문제를 풀 때는 포인터를 움직여서 찾는걸 했는데, 이렇게 찾으면 값에 따라서 빵구가 날 수 있다. 그래서 왼쪽 포인터와 오른쪽 포인터를 잡아서 가운데 값으로 찾아가도록 해서 문제 해결!