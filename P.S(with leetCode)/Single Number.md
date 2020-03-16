### Single Number



Today's Problem is Easy - [Single Number](https://leetcode.com/problems/single-number/submissions/)

### Problem

Given a non-empty array of integers, every element appears twice except for one. Find that single one.

Note:

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

Example 1:

```
Input: [2,2,1]
Output: 1
```

Example 2:

```
Input: [4,1,2,1,2]
Output: 4
```

### Solution

```
class Solution {
    fun singleNumber(nums: IntArray): Int {
        var sum = 0
        for (i in 0 until nums.size) {
            sum = sum xor nums[i]
        }
        return sum
    }
}
```

### Explanation

Easy문제는 대체로 특정 개념에 대해서 아는가에 대한 질문이 많다. 이 문제도 해시맵을 사용하면 그냥 바로 해결 할 수 있지만, extra memory를 사용하지 않고 하려면 xor의 존재를 깨달아야한다..! 오늘도 또 배웠다.