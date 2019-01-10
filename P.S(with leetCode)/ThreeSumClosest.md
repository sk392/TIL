### ThreeSumClosest


요번엔 [ThreeSumClosest](https://leetcode.com/problems/3sum-closest/)요거를 풀어봐씁니다.

### Problem

Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

###### Example:

```
Given array nums = [-1, 2, 1, -4], and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```
### Solution

```
import kotlin.math.abs

class Solution {
    fun threeSumClosest(nums: IntArray, target: Int): Int {
            nums.sort()
        var result: Int? = null
        for (i in 0 until nums.size) {
            var left = i + 1
            var right = nums.size - 1
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue
            }

            while (left < right) {
                val sum = nums[i] + nums[left] + nums[right]
                when {
                    sum == target -> {
                        return target
                    }
                    sum < target -> {
                        if (result == null || abs(target - result) > abs(target - sum)) {
                            result = sum
                        }
                        left++
                    }
                    sum > target -> {
                        if (result == null || abs(target - result) > abs(target - sum)) {
                            result = sum
                        }
                        right--
                    }

                }
            }
        }
        return result ?: 0
    }
}
```

### Explanation

3Sum과 비슷한 방식으로 해결했다. 먼저 sort를 해주고, for문으로 1개를 잡고 왼쪽과 오른쪽 끝부터 해서 계산하도록해서 O(n^2)으로 해결!

p.s 음 속도가 하위 22퍼 나왔는데, 다른 개선안이 존재하는 것같다. 추후 생각!
