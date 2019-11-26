### First Missing Positive



오늘의 문제는! Hard - [First Missing Positive](https://leetcode.com/problems/first-missing-positive/)

### Problem

Given an unsorted integer array, find the smallest missing positive integer.

Example 1:

```
Input: [1,2,0]
Output: 3
```

Example 2:

```
Input: [3,4,-1,1]
Output: 2
```

Example 3:

```
Input: [7,8,9,11,12]
Output: 1
```

Note:

Your algorithm should run in O(n) time and uses constant extra space.


### Solution

```
class Solution {
    fun firstMissingPositive(nums: IntArray): Int {
        val size = nums.size

        for (i in 0 until size) {
            var num = nums[i]
            if (num > size || num <= 0) {
                nums[i] = size + 1
            }
        }

        for (i in 0 until size) {
        var num = Math.abs(nums[i])
            if (num == size + 1) {
                continue
            }

            num--
            if (nums[num] > 0) {
                nums[num] *= -1
            }
        }
        for (i in 0 until size) {
            if (nums[i] > 0) {
                return i + 1
            }
        }

        return size + 1
    }
}

```

### Explanation

음.. 일단 문제는 1부터 nums의 사이즈만큼을 전부 넣고 거기서 nums에 값들을 각각 리무브한 후에 최솟값을 찾으면 쉬운데, 이건 공간을 N만큼 추가로 쓰게된다.

그래서 다른 방법으로 음수거나 사이즈보다 큰 값들을 모두 size +1값으로 매핑해주고 해당 매핑 값이 아닌 경우는 그 값으로 인덱스를 찾아서 해당 인덱스의 값을 -로 전환해준다. 그후에 마지막으로 돌면서 0보다 큰 값을 찾으면 해결!@