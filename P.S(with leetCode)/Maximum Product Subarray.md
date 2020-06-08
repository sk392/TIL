### Maximum Product Subarray



Today's Problem is Medium - [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

### Problem

Given an integer array nums, find the contiguous subarray within an array (containing at least one number) which has the largest product.

Example 1:

```
Input: [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
```

Example 2:

```
Input: [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```

### Solution

```
class Solution {

    fun maxProduct(nums: IntArray): Int {
        if(nums.isEmpty()) return 0

        var min = nums[0]
        var max = nums[0]
        var result = nums[0]

        for( i in 1 until nums.size){
            val tempMax = Math.max(nums[i],Math.max(max * nums[i],min* nums[i]))
            val tempMin =Math.min(nums[i],Math.min(max * nums[i],min* nums[i]))
            max = tempMax
            min = tempMin
            result = Math.max(result,Math.max(max,min))
        }

        return result
    }
}
```

### Explanation

한동안 안풀었더니.. 감이 많이 떨어진 것같다. 

결과를 얻으려면 가장 낮은 값과 가장 높은 값을 동시에 가져가야 한다. 곱의 성질을 고려해봤을 때, 가장 낮은 값에 다시 음수를 곱한다면 가장 큰 값이 될 수 있으므로. 가장 낮은 값과 가장 큰 값을 가지고 다니면서 결과를 갱신해간다.