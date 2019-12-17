### Largest Rectangle in Histogram


Today's Problem  Hard - [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

### Problem

Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

 

![](https://assets.leetcode.com/uploads/2018/10/12/histogram.png)

```
Above is a histogram where width of each bar is 1, given height = [2,1,5,6,2,3].
```
 

![](https://assets.leetcode.com/uploads/2018/10/12/histogram_area.png)

```
The largest rectangle is shown in the shaded area, which has area = 10 unit.
```
 

Example:

```
Input: [2,1,5,6,2,3]
Output: 10
```


### Solution

```
class Solution {
    fun largestRectangleArea(heights: IntArray): Int {
        if (heights.isEmpty()) return 0

        var result = heights[0]
        for (i in 0 until heights.size) {
            var left = i
            var right = i
            val height = heights[i]

            while (left >= 0 && heights[left] >= height) left--
            while (right < heights.size && heights[right] >= height) right++
            result = Math.max(result, ((right - 1) - (left + 1) + 1) * height)

        }

        return result
    }
}
```

### Explanation

역시 난이도가 하드라서 그런지 못풀었다. left, right포지션 값을 찾아서 right를 증가하거나 left도 증가하는 로직을 통해서 최적화된 값을 찾으려고 했는데, 이런 방법은 right가 언제 증가해야하는지, left는 언제 증가해야하는지에 대한 처리가 까다로웠고, 결국 모든 해를 포함하는 조건은 찾지 못했다.

Discuss에서 여러 해중에서 나와 비슷한 컨셉으로 해결한 솔루션을 찾았고, 비슷한 컨셉으로 해결한 솔루션이 기억에 오래 남을 것같다는 생각을 했다.

솔루션은 각 좌표에서 시작해서 왼쪽과 오른쪽으로 넓혀가는 코드인데, 이 코드의 조건을 보면 현재값보다 다음 값이 높거나 같을 때만 찾는데, 그렇다면 값이 작은 솔루션은 어떻게 해결하냐는 의문이 남을 수있는데, 잘 보면 모든 값을 기준으로 최대 크기의 사각형을 만듦으로써 그런 부분을 커버할 수 있게된다.