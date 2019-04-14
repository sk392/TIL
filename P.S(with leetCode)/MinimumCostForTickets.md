### MinimumCostForTickets



요번엔 [MinimumCostForTickets](https://leetcode.com/problems/minimum-cost-for-tickets/)요거를 풀어봐씁니다.

### Problem

In a country popular for train travel, you have planned some train travelling one year in advance.  The days of the year that you will travel is given as an array days.  Each day is an integer from 1 to 365.

Train tickets are sold in 3 different ways:

a 1-day pass is sold for costs[0] dollars;
a 7-day pass is sold for costs[1] dollars;
a 30-day pass is sold for costs[2] dollars.
The passes allow that many days of consecutive travel.  For example, if we get a 7-day pass on day 2, then we can travel for 7 days: day 2, 3, 4, 5, 6, 7, and 8.

Return the minimum number of dollars you need to travel every day in the given list of days.

 
```
Example 1:

Input: days = [1,4,6,7,8,20], costs = [2,7,15]
Output: 11
Explanation: 
For example, here is one way to buy passes that lets you travel your travel plan:
On day 1, you bought a 1-day pass for costs[0] = $2, which covered day 1.
On day 3, you bought a 7-day pass for costs[1] = $7, which covered days 3, 4, ..., 9.
On day 20, you bought a 1-day pass for costs[0] = $2, which covered day 20.
In total you spent $11 and covered all the days of your travel.

```

```
Example 2:

Input: days = [1,2,3,4,5,6,7,8,9,10,30,31], costs = [2,7,15]
Output: 17
Explanation: 
For example, here is one way to buy passes that lets you travel your travel plan:
On day 1, you bought a 30-day pass for costs[2] = $15 which covered days 1, 2, ..., 30.
On day 31, you bought a 1-day pass for costs[0] = $2 which covered day 31.
In total you spent $17 and covered all the days of your travel.
 
 ```
 
```
Note:

1 <= days.length <= 365
1 <= days[i] <= 365
days is in strictly increasing order.
costs.length == 3
1 <= costs[i] <= 1000
```


### Solution

```
class Solution {
    fun mincostTickets(days: IntArray, costs: IntArray): Int {
        val dp = IntArray(days.size)

        for (i in days.indices) {
            var passDay = costs[0]
            var passWeek = costs[1]
            var passMonth = costs[2]

            for (j in i - 1 downTo 0) {
                if (passDay == costs[0] && days[j] <= days[i] - 1) passDay += dp[j]

                if (passWeek == costs[1] && days[j] <= days[i] - 7) passWeek += dp[j]

                if (passMonth == costs[2] && days[j] <= days[i] - 30) passMonth += dp[j]
            }

            dp[i] = Math.min(passDay, Math.min(passWeek, passMonth))
        }

        return dp[days.size - 1]
    }
}
```

### Explanation

days의 날 하나하나 마다 최소값을 구한 후, 해당 최소값을 통해 다음 days의 날에 더해준다. DP로 풀음!