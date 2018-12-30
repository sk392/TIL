###  MaximumSubarray


요번엔 [MaximumSubarray](https://leetcode.com/problems/maximum-subarray/submissions/)요거를 풀어봐씁니다.

### Problem
Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

Example:

```
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.

```

Follow up:

If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.

### Solution

```
class Solution {
    fun maxSubArray(nums: IntArray): Int {
            var maxValue = Int.MIN_VALUE
        var sum = 0
        nums.forEach { value ->
            sum += value

            if(sum >maxValue) maxValue = sum
            if(sum<=0) sum = 0
        }
        return maxValue
    }
}
```

### Explanation

시작 값은 반드시 양수로 시작해야하고.. 이전까지의 합이 0보다 적다면 그 전까지의 합을 버리고 새로 계산을 시작한다. 그 중에 최고 값이 있다면 max값을 반환 해주면 된당... 으으.. 이런건 규칙 찾기가 너무 힘든 것같다 ㅠㅠ

