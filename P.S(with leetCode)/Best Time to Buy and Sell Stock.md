### Best Time to Buy and Sell Stock



Today's Problem is Easy - [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

### Problem

Say you have an array for which the ith element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

Example 1:

```
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.
```

Example 2:

```
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```

### Solution

```
class Solution {
    fun maxProfit(prices: IntArray): Int {
        var maxProfit = 0
        var buyPrice = Int.MAX_VALUE

        for (i in 0 until prices.size) {
            buyPrice = Math.min(buyPrice, prices[i])
            maxProfit = Math.max(maxProfit, prices[i] - buyPrice)
        }

        return maxProfit
    }
}
```

### Explanation

처음엔 이전값보다 다음값이 높을 떄만 (그래야 이익을 볼 수 있으니) 구매한 후에 뒤에서 팔았는데, 굳이 그렇게 if문을 넣을 필요 없이현재 값을 매번 미니멈 값으로 갱신한 후에 맥스 값을 계산하면 된다!

> P.S - Profit과 Benefit에 고민하다가 찾았는데 Profit이 맞는 워딩인 것같다!