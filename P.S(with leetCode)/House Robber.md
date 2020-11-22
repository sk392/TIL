### House Robber



Today's Problem is EASY - [House Robber](https://leetcode.com/problems/house-robber/)

### Problem



You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

 

Example 1:

```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.
```

Example 2:

```
Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
             Total amount you can rob = 2 + 9 + 1 = 12.
``` 

Constraints:

```
0 <= nums.length <= 100
0 <= nums[i] <= 400
```

### Solution

```
class Solution {
    val robRecursiveMap = mutableMapOf<Int,Int>()

    fun rob(nums: IntArray): Int {
        return Math.max(robRecursive(nums, 0), robRecursive(nums, 1))
    }
    
    fun robRecursive(nums: IntArray, index: Int): Int {
        if (index >= nums.size) return 0

        robRecursiveMap[index+2] = robRecursiveMap[index+2] ?: robRecursive(nums,index+2)
        robRecursiveMap[index+3] = robRecursiveMap[index+3] ?: robRecursive(nums,index+3)

        return nums[index] + Math.max(robRecursiveMap[index+2]!!,robRecursiveMap[index+3]!!)
    }
}
```

### Explanation

해당 문제에 접근할 때 먼저 재귀함수가 생각났다. 
0번쨰나 1번째로 선택한값으로 시작해서 (각 엘리먼트가 양의 변수이므로 2번째 값을 선택하면 반드시 0번째 값을 추가할 수 있으니 명확한 구분을 위해 0,1번째만 나눈다) 연속될 수 없으니 한칸 뛴 +2번쨰 인덱스나 혹은 +3번째 인덱스를 포함한 재귀로 재요청한다(+4번째 인덱스는 처음과 마찬가지로 +2번째 인덱스를 반드시 선택할 수 있으니 잘못된 접근이다.)