### Linked List Cycle II



Today's Problem is Medium - [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

### Problem

Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.

Note: Do not modify the linked list.

 

Example 1:

```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```
![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

Example 2:

```
Input: head = [1,2], pos = 0
Output: tail connects to node index 0
Explanation: There is a cycle in the linked list, where tail connects to the first node.
```
![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

Example 3:

```
Input: head = [1], pos = -1
Output: no cycle
Explanation: There is no cycle in the linked list.
```

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)
 

Follow-up:

```
Can you solve it without using extra space?
```
### Solution

```
/**
 * Example:
 * var li = ListNode(5)
 * var v = li.`val`
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int) {
 *     var next: ListNode? = null
 * }
 */

class Solution {
    fun detectCycle(head: ListNode?): ListNode? {
        var fast: ListNode? = head
        var slow: ListNode? = head
        while (fast?.next != null) {
            slow = slow?.next
            fast = fast.next?.next

            if (fast == slow) {
                fast = head
                while (slow != fast) {
                    slow = slow?.next
                    fast = fast?.next
                }
                return slow
            }
        }
        return null
    }
}
```

### Explanation

fast와 slow가 첫번째 만나는 회수가 바로 사이클의 사이즈가 되고, 그 이후 slow와 head에서 한칸씩이동해서 만나는 순간이 바로 사이클의 헤드로 지정됩니다!