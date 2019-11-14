### Container With Most Water



오늘의 문제는! 난이도 Medium의 [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

### Problem

Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and n is at least 2.

 ![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

 

Example:

```
Input: [1,8,6,2,5,4,8,3,7]
Output: 49
```

### Solution

```
class Solution {
    fun maxArea(height: IntArray): Int {
        var result = 0

        for (i in 0 until height.size) {
                if (height[i] * (height.size - i) < result) continue
            for (j in i + 1 until height.size) {

                val area = (j - i) * Math.min(height[i], height[j])

                if (area > result) {
                    result = area
                }
            }
        }

        return result
    }
}
```

### Explanation

우선은 가장 간단하게 왼쪽부터 하나의 포지션(i)를 잡고 오른쪽으로 비교해나가면서 그 중에 가장 큰 값을 올리면댄당 그럼 빅오로 n^2가 나올텐딩... 먼가 더 나은 방법이 있을까 싶긴한데 일단 튜닝을 목적으루 현재 i값이 가질 수 있는 최대값(i * size-i)를 계산해서 max보다 낮다면 포문을 돌리지 않도록 했다.