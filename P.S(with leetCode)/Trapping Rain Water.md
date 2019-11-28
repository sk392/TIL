### Trapping Rain Water



Today's ProblemSolving -Hard- [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

### Problem

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.


![](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)
The above elevation map is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped. Thanks Marcos for contributing this image!

Example:

```
Input: [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
```

### Solution

```
import java.util.*

class Solution {
    var resultTrapping = 0

    fun trap(height: IntArray): Int {
        val heights = ArrayDeque<Pair<Int, Int>>()

        for (i in 0 until height.size) {
            val current = height[i]
            if (i - 1 >= 0) {
                val prev = height[i - 1]

                if (current > prev) {
                    while (!heights.isEmpty() && heights.peek().second <= current) {
                        trapping(heights.pop(), i, current, height)
                    }
                    if (!heights.isEmpty()) {
                        trapping(heights.peek(), i, current, height)
                    }
                }
            }

            if (current > 0) {
                heights.push(Pair(i, current))
            }
        }
        return resultTrapping
    }

    fun trapping(temp: Pair<Int, Int>, i: Int, current: Int, height: IntArray) {
        val min = Math.min(temp.second, current)

        for (j in temp.first..i) {
            val value = min - height[j]
            if (value > 0) {
                resultTrapping += value
                height[j] = min
            }
        }
    }
}
```

### Explanation

음.. 문제를 보고 최근에 풀었던 문제중에 스택으로 풀었던게 생각나서 스택을 활용해봤다. 빗물이 고이기 위해서는 양쪽에 페어 형태로 기둥이 존재해야하고, 해당 쌍을 찾는 것을 스택을 활용했다.

0보다 큰 값을 가진(높이가 존재하는) 값을 스택에 넣고, 이전 값보다 현재 값이 높으면 높이가 높아졌다는걸 의미하고 또 다른 말로는 비가 고일 수 있는 가능성이 있다는걸 의미한다. 그래서 그 때 스택에서의 값을 보고 현재 값보다 작거나 같은게 있으면 없어질 때까지 비가 고일 수 있고, 전부 계산하고 나면 이전에 있는 높이가 현재보다 높은 경우도 카운트 해줘야한다. 주의할점은 이 때는 pop을 하면안되고 peek을 해야한다. pop을 하면서 더 높게 채울 수 있는 가능성을 없애고, peek을 한다고 하더라도 연산이 더 늘어날 순 있지만 정답을 벗어나지 않기때문에 이렇게 해야 깔끔하게 할 수 있다.