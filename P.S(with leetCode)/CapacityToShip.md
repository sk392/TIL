### Capacity To Ship Packages Within D Days



요번엔 [Capacity To Ship Packages Within D Days
](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days//)요거를 풀어봐씁니다.

### Problem

A conveyor belt has packages that must be shipped from one port to another within D days.

The i-th package on the conveyor belt has a weight of weights[i].  Each day, we load the ship with packages on the conveyor belt (in the order given by weights). We may not load more weight than the maximum weight capacity of the ship.

Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within D days.

 
```
Example 1:

Input: weights = [1,2,3,4,5,6,7,8,9,10], D = 5
Output: 15
Explanation: 
A ship capacity of 15 is the minimum to ship all the packages in 5 days like this:
1st day: 1, 2, 3, 4, 5
2nd day: 6, 7
3rd day: 8
4th day: 9
5th day: 10

Note that the cargo must be shipped in the order given, so using a ship of capacity 14 and splitting the packages into parts like (2, 3, 4, 5), (1, 6, 7), (8), (9), (10) is not allowed. 
```

```
Example 2:

Input: weights = [3,2,2,4,1,4], D = 3
Output: 6
Explanation: 
A ship capacity of 6 is the minimum to ship all the packages in 3 days like this:
1st day: 3, 2
2nd day: 2, 4
3rd day: 1, 4
```

```
Example 3:

Input: weights = [1,2,3,1,1], D = 4
Output: 3
Explanation: 
1st day: 1
2nd day: 2
3rd day: 3
4th day: 1, 1
```
 
```
Note:

1 <= D <= weights.length <= 50000
1 <= weights[i] <= 500
```

### Solution

```
class Solution {
    fun shipWithinDays(weights: IntArray, D: Int): Int {
        var minValue = 1
        var maxValue = 500 * weights.size 
        while (minValue <= maxValue) {
            val mid = (minValue + maxValue) / 2
            if (isEnough(weights, mid, D)) {
                maxValue = mid - 1
            } else {
                minValue = mid + 1
            }
        }
        return minValue
    }

    private fun isEnough(weights: IntArray, load: Int, D: Int): Boolean {
        var day = 0
        var sum = 0
        for (i in weights.indices) {
            if (weights[i] > load) {
                return false
            }

            sum += weights[i]
            if (sum > load) {
                sum = weights[i]
                day++
            }
        }
        return day < D
    }
}
```

### Explanation

문제는 몇척의 배가 필요하냐인데, 결국은 다 해보는 수밖에 없다 풀써치를 해야되는데 풀써치를 줄이기 위해서 Min, Max값을 설정해서 확인해야하는 값을 줄이기 위해 Min은 각 항목별 최고값, Max는 weights.size를 D로나눠서 해당 값으로 해놨는데... 잘못 생각했던 것같다. 그래서 Discuss를 봤는데, 해당 값의 맥스 값을 각 항목 별 최대값(500)에 항목 개수를 곱해서 구해서 바이너리 서치로 찾아갔다. kotlin버전 디스커즈는 없길래 올려둠 ㅋ