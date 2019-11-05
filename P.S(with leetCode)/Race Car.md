### Race Car


요번엔 [Race Car](https://leetcode.com/problems/all-paths-from-source-to-target/)요거를 풀어봐씁니다.

### Problem

Your car starts at position 0 and speed +1 on an infinite number line.  (Your car can go into negative positions.)

Your car drives automatically according to a sequence of instructions A (accelerate) and R (reverse).

When you get an instruction "A", your car does the following: position += speed, speed *= 2.

When you get an instruction "R", your car does the following: if your speed is positive then speed = -1 , otherwise speed = 1.  (Your position stays the same.)

For example, after commands "AAR", your car goes to positions 0->1->3->3, and your speed goes to 1->2->4->-1.

Now for some target position, say the length of the shortest sequence of instructions to get there.


Example 1:

```
Input: 
target = 3
Output: 2
Explanation: 
The shortest instruction sequence is "AA".
Your position goes from 0->1->3.
```


Example 2:

```
Input: 
target = 6
Output: 5
Explanation: 
The shortest instruction sequence is "AAARA".
Your position goes from 0->1->3->7->7->6.
``` 

Note:

```
1 <= target <= 10000.
```

### Solution

```
class Solution {
    fun racecar(target: Int): Int {
        val dp = IntArray(target + 1)
        dp[0] = 0

        for (i in 1..target) {

            var rightPosition = 0
            var rightSteps = 0
            while (rightPosition < i) {
                rightSteps++
                rightPosition = (1 shl rightSteps) - 1
            }
            if (rightPosition == i) {
                dp[i] = rightSteps
                continue
            }

            dp[i] = rightSteps + 1 + dp[rightPosition - i]


            val leftSteps = rightSteps - 1
            val leftPosition = (1 shl leftSteps) - 1

            for (j in 0 until leftSteps) {
                val reversePosition = leftPosition - ((1 shl j) - 1)
                dp[i] = Math.min(dp[i], leftSteps + 1 + j + 1 + dp[i - reversePosition])
            }
        }
        return dp[target]
    }

}
```

### Explanation

음.. 이 문제를 푸는데 2일이나 걸렸는데, 혼자 힘으로 못풀어서 결국 Disc
uss의 힘을 빌어 풀었다. DP와 BFS형태로 풀 수 있는데, 자주 사용하던 DP의 해결방법인 재귀를 통해 해결하는 것은 StackOverFlow가 발생한다.  (이걸 해결하기 위해 튜닝을 오지게했지만 실패..ㅠㅠ)
 솔루션은 1부터 target지점까지의 모든 거리에 대해 최소값을 구하며 앞으로 나아간다. target 지점의 2의 배승의 합만큼의 거리(특정 지점에 도달하는데 최소의 값) 중에 가장 가까운 거리를 계산해서 rightPosition을 잡는다. rightStep + 1(reverse) + dp[rightPosition -i]의 값 즉, 오른쪽에서 다시 왼쪽으로 돌아가는 방법과, leftPosition을 잡아서 왼쪽에서 리버스로 j만큼 스탭을 밟은 후에 다시 오른쪽으로 가는 방법 중에 작은 값으로 결과를 얻게된다.