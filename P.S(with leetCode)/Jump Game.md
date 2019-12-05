### Jump Game


Today's Problem Medium-  [Jump Game](https://leetcode.com/problems/jump-game/)

### Problem

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

Example 1:

```
Input: [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

Example 2:

```
Input: [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum
             jump length is 0, which makes it impossible to reach the last index.
```
             
              
### Solution

```
class Solution {
    fun canJump(nums: IntArray): Boolean {
        val size = nums.size
        var maximumJumpIndex = 0

        for (i in 0 until size) {
            if (i > maximumJumpIndex) break

            maximumJumpIndex = Math.max(maximumJumpIndex, i + nums[i])
        }

        return maximumJumpIndex >= size - 1
    }
}
```

### Explanation

맨처음엔 booleanArray를 하나 쓸까 하다가 어차피 최고로 점프가능한 인덱스만 알면 되겠다 싶어서 maximumJumpIndex를 구해가지고 해당 인덱스보다 크면 중단하는 형태로 진행했다.