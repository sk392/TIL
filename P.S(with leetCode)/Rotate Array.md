### Rotate Array


Today's Problem is Medium - [Rotate Array](https://leetcode.com/problems/rotate-array/)

### Problem

Given an array, rotate the array to the right by k steps, where k is non-negative.


 

Example 1:

```
Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]
```

Example 2:

```
Input: nums = [-1,-100,3,99], k = 2
Output: [3,99,-1,-100]
Explanation: 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]
```

Constraints:

```
1 <= nums.length <= 105
-231 <= nums[i] <= 231 - 1
0 <= k <= 105
```


Follow up:

* Try to come up with as many solutions as you can. There are at least three different ways to solve this problem.
* Could you do it in-place with O(1) extra space?


### Solution

```kotlin

class Solution {

    fun rotate(nums: IntArray, k: Int): Unit {
        val newK = if(k > nums.size) k % nums.size
        else k
        rotateInternal(nums,0, newK)
    }

    fun rotateInternal(nums: IntArray, index: Int, k: Int) {
        if (index >= nums.size) return
        val newIndex = if (index - k >= 0) {
            index - k
        } else {
            index - k + nums.size
        }

        val newValue = nums[newIndex]
        rotateInternal(nums,index+1 , k)
        nums[index] = newValue
    }

}
```

### Explanation

rotate하는 방법은 간단하다, 우선은 bruteForce하도록 하려면 배열을 하나 더 만든 후에 k만큼 이동하면되는데, follow up에서 제시한 것처럼 추가 메모리 사용 없이 하는 것이 조금 까다롭다. 또 신경써야할 점은 k가 충분히 큰 숫자가 가능하기 때문에 size보다 길 경우에 대해 불필요한 연산을 많이 하지 않도록 해야한다.