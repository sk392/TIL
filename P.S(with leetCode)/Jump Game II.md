### Jump Game II



Today's Problem is Hard - [Jump Game II](https://leetcode.com/problems/jump-game-ii/)

### Problem


Given an array of non-negative integers nums, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

You can assume that you can always reach the last index.


Example 1:

```
Input: nums = [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

Example 2:

```
Input: nums = [2,3,0,1,4]
Output: 2
```
 

Constraints:

```
1 <= nums.length <= 3 * 104
0 <= nums[i] <= 105
```


### Solution

```
class Solution {
    fun jump(nums: IntArray): Int {
        
        var count = 0
        var curEnd = 0
        var curFarthest = 0
        
        for (i in 0 until nums.size -1) {
            curFarthest = Math.max(curFarthest, i + nums[i])
            if (i == curEnd) {
                count++
                curEnd = curFarthest
            }
        }
        
        return count
    }
}
```

### Explanation

오우.. 처음엔 당연히 visitMap을 만들어서 방문했으면 방문하지 않게 booleanArray를 만들어서 풀었는데, 이렇게되면 최저 횟수를 체크할 수 없다. 그래서 IntArray로 바꾸고 해당 index의 값보다 높으면 해당 방문은 그상태로 return하도록 했더니.. 타임 오버가 떴다.
그래서 1..MAX -> MAX DownTO 1로 포문을 바꿨더니 시간안에 들어오게 되었고. 가장 빠른 방법 이뭔지 보고 엄청 감탄했다.

솔루션에 적어둔게 그 N으로 풀게된 가장 빠른 방법인데, 문제의 방향성을 뒤집어서 풀었다. 내 시점은 해당끝좌표에 가장 적은 카운트로 도착하는 것을 포커싱헀는데, 솔루션에선 해당 카운트 내에 가장 멀리 갈 수 있는 인덱스를 찾아서 문제를 해결했다. 
현재 카운트 내에 가장 멀리 갈 수 있는 값(curFarthest)을 찾아서 현재 카운트에서 가장 먼 곳(curEnd)에 도착하면 카운트를 하나 늘려주고 현재 가장 도착한 먼 곳을 갱신해서 이 루틴을 반복하게 된다. 멋있다.