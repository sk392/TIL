### Hand of Straights



요번엔 [Hand of Straights](https://leetcode.com/problems/hand-of-straights/)요거를 풀어봐씁니다.

### Problem

Alice has a hand of cards, given as an array of integers.

Now she wants to rearrange the cards into groups so that each group is size W, and consists of W consecutive cards.

Return true if and only if she can.

 
```
Example 1:

Input: hand = [1,2,3,6,2,3,4,7,8], W = 3
Output: true
Explanation: Alice's hand can be rearranged as [1,2,3],[2,3,4],[6,7,8].
```

```
Example 2:

Input: hand = [1,2,3,4,5], W = 4
Output: false
Explanation: Alice's hand can't be rearranged into groups of 4.
 ```
 
```
Note:

1 <= hand.length <= 10000
0 <= hand[i] <= 10^9
1 <= W <= hand.length
```


### Solution

```
import java.util.LinkedList

class Solution {
    fun isNStraightHand(hand: IntArray, W: Int): Boolean {
        if (W == 1) return true
        if (hand.size % W != 0) return false
        val n = hand.size
        hand.sort()
        val q = LinkedList<Int>()
        var i = 0
        while (i < n) {
            if (!q.isEmpty() && hand[i] != hand[i - 1] + 1) return false
            var repeat = 0
            val v = hand[i]
            while (repeat == 0 || i < n && hand[i] == hand[i - 1]) {
                ++repeat
                ++i
            }
            while (q.size < repeat) q.offer(v)
            while (!q.isEmpty() && v - q.peek() + 1 === W) q.poll()
        }
        return q.isEmpty()
    }
}
```

### Explanation

간단하게 풀어보아따!