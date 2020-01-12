### Find Pivot Index


Today's Problem is Easy - [Find Pivot Index](https://leetcode.com/problems/find-pivot-index/)


### Problem

Given an array of integers nums, write a method that returns the "pivot" index of this array.

We define the pivot index as the index where the sum of the numbers to the left of the index is equal to the sum of the numbers to the right of the index.

If no such index exists, we should return -1. If there are multiple pivot indexes, you should return the left-most pivot index.

Example 1:

```
Input: 
nums = [1, 7, 3, 6, 5, 6]
Output: 3
Explanation: 
The sum of the numbers to the left of index 3 (nums[3] = 6) is equal to the sum of numbers to the right of index 3.
Also, 3 is the first index where this occurs.
``` 

Example 2:

```
Input: 
nums = [1, 2, 3]
Output: -1
Explanation: 
There is no index that satisfies the conditions in the problem statement.
``` 

Note:

```
The length of nums will be in the range [0, 10000].
Each element nums[i] will be an integer in the range [-1000, 1000].
```

Note:

The number of nodes in the tree will be between 2 and 100.
Each node has a unique integer value from 1 to 100.

### Solution

```
class Solution {
    fun pivotIndex(nums: IntArray): Int {
        var sum = 0
        var subSum = 0

        for (i in 0 until nums.size) {
            sum += nums[i]
        }
        for (i in 0 until nums.size) {
            if((subSum * 2 + nums[i]) == sum) return i
            subSum += nums[i]
        }
        return -1
    }
}

```

### Explanation

맨처음엔 subsum을 넣어두는 맵을 사용했는데, 어차피 이렇게해도 똑같은 수가 돌아서 ~~(그리고 틀려버렸다;)~~ 그래서 간단하게 subSum을 구해서 sum과 같은지 판단하고 해결!