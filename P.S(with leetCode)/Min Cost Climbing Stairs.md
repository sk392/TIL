### Min Cost Climbing Stairs

요번엔 [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)요거를 풀어봐씁니다.

### Problem
On a staircase, the i-th step has some non-negative cost cost[i] assigned (0 indexed).

Once you pay the cost, you can either climb one or two steps. You need to find minimum cost to reach the top of the floor, and you can either start from the step with index 0, or the step with index 1.

Example 1:

```
Input: cost = [10, 15, 20]
Output: 15
Explanation: Cheapest is start on cost[1], pay that cost and go to the top.
```

Example 2:

```
Input: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
Output: 6
Explanation: Cheapest is start on cost[0], and only step on 1s, skipping cost[3].
```

Note:

```
cost will have a length in the range [2, 1000].
Every cost[i] will be an integer in the range [0, 999].
```


### Solution

```
class Solution {
    val minCostClimbingStairsMap = HashMap<Int,Int>()

    fun minCostClimbingStairs(cost: IntArray): Int {
        val minArray = MutableList(cost.size){0}
        cost.forEachIndexed{index, value ->
            when (index) {
                0 -> {minArray[index] = cost[0]}
                1 -> {minArray[index] = cost[1]}
                else -> {
                    minArray[index] = Math.min(minArray[index-1],minArray[index-2])+ value
                }
            }
        }
        return Math.min(minArray[cost.size-1],minArray[cost.size-2])
    }
}
```

### Explanation

문제 풀이는 지금 현재 계단까지 오는데 최소 값을 구하고, 마지막 계단과 마지막 전 계단에서 최소값을 구하면 된다. 맨첨에는 재귀로 풀었으나, 재귀는 타임아웃이 걸려서 for문을 통해서 현재 위치에 올 수 있는 계단의 최솟값에서 현재 계단 값을 더해서 해결!