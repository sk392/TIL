### Triangle

요번엔 [Triangle](https://leetcode.com/problems/triangle/)요거를 풀어봐씁니다.

### Problem
Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

For example, given the following triangle

```
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
The minimum path sum from top to bottom is 11 (i.e., 2 + 3 + 5 + 1 = 11).
```

Note:

```
Bonus point if you are able to do this using only O(n) extra space, where n is the total number of rows in the triangle.
```


### Solution

```
import java.util.*
import kotlin.math.min
class Solution {

    fun minimumTotal(triangle: List<List<Int>>): Int {

        var resultList = IntArray(triangle.size)
        resultList[0] = triangle[0][0]

        for (i in 1 until triangle.size) {
            val list = triangle[i]
            val tempResultList = resultList.clone()
            for (j in 0 until list.size) {
                tempResultList[j] = getMinimumValue(j, Arrays.copyOfRange(resultList,0,i)) + list[j]
            }
            resultList = tempResultList.clone()
        }

        return resultList.min() ?: 0
    }

    fun getMinimumValue(p: Int, list: IntArray): Int {
        var left = Int.MAX_VALUE
        var right = Int.MAX_VALUE
        if (p - 1 >= 0) {
            left = list[p - 1]
        }
        if (p < list.size) {
            right = list[p]
        }
        return min(left, right)
    }
}
```

### Explanation

이제 이런 계단식형 문제는 쉽게 풀 수 있다. 관점만 잘 기억하면되는데,  현재 단계에서 다음단계를 어떻게 나아갈건지 보는게 아니라, 다음 단계 기준으로 내 계단으로 올 수 있는 경우의 수 중에 제일 적은 수를 더해서 현재 단계 현재칸에 올 수 있는 최소값을 구하고 마지막 라인에서 제일 적은 값을 구하면된다.