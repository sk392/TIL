### FourSum

요번엔 [FourSum](https://leetcode.com/problems/4sum/)요거를 풀어봐씁니다.

### Problem
Given an array nums of n integers and an integer target, are there elements a, b, c, and d in nums such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.

Note:

The solution set must not contain duplicate quadruplets.

Example:

```
Given array nums = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]

```

### Solution

```
class Solution {
    fun fourSum(nums: IntArray, target: Int): List<List<Int>> {

        nums.sort()
        val result = mutableListOf<List<Int>>()

        for (i in 0 until nums.size) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue
            }
            for (j in i + 1 until nums.size) {
                if (j > i + 1 && nums[j] == nums[j - 1]) {
                    continue
                }
                var left = j + 1
                var right = nums.size - 1

                while (left < right) {
                    val sum = nums[i] + nums[j] + nums[left] + nums[right]

                    when {
                        sum == target -> {
                            result.add(listOf(nums[i], nums[j], nums[left], nums[right]))
                            while(left <right && nums[left] == nums[left+1]) left++
                            while(left <right && nums[right] == nums[right-1]) right--

                            right--
                            left++
                        }
                        sum > target -> right--

                        sum < target -> left++
                    }
                }
            }
        }
        return result
    }
}
```

### Explanation

3Sum에서 문제를 못풀고 솔루션을 참고했었어서 4Sum도 아마 비슷한 방식으로 풀이할 수 있을 것같아서 내 손으로 풀어보았다!

컨셉은 역시 같다. sort를 하고 이중 포문 안에서 특정 값(target : Int)보다 작으면 left(음수)를 감소시키고 작으면 right(양수)를 증가해서 문제를 해결할 수 있고 그렇게 되면 n^3으로 문제를 해결 할 수 있다.