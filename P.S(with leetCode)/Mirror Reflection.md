###   Mirror Reflection


요번엔 [ Mirror Reflection](https://leetcode.com/problems/mirror-reflection/)요거를 풀어봐씁니다.

### Problem
here is a special square room with mirrors on each of the four walls.  Except for the southwest corner, there are receptors on each of the remaining corners, numbered 0, 1, and 2.

The square room has walls of length p, and a laser ray from the southwest corner first meets the east wall at a distance q from the 0th receptor.

Return the number of the receptor that the ray meets first.  (It is guaranteed that the ray will meet a receptor eventually.)


```
Example 1:

Input: p = 2, q = 1
Output: 2
Explanation: The ray meets receptor 2 the first time it gets reflected back to the left wall.
```
![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/06/18/reflection.png)


```
Note:

1 <= p <= 1000
0 <= q <= p
Accepted
6,323
Submissions
12,164
```


### Solution

```
class Solution {
    fun mirrorReflection(p: Int, q: Int): Int {

        val t = tail(p, q)

        val a = p / t % 2
        val b = q / t % 2

        return if (a == 1) {
            a * b
        } else {
            2
        }
    }

    private fun tail(p: Int, q: Int): Int {
        return if (q == 0) {
            p
        } else tail(q, p % q)
    }
}
```

### Explanation

간단한 문제.. 약간 코딩테스트보다는 수학 느낌이 있었지만ㅋㅋㅋㅋ