### Reverse Nodes in k-Group

Hard - [Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

### Problem

Given the head of a linked list, reverse the nodes of the list k at a time, and return the modified list.

k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

Example 1:

![](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg)

Input: head = [1,2,3,4,5], k = 2
Output: [2,1,4,3,5]

Example 2:

![](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg)

Input: head = [1,2,3,4,5], k = 3
Output: [3,2,1,4,5]

Constraints:

The number of nodes in the list is n.
1 <= k <= n <= 5000
0 <= Node.val <= 1000

### Solution

```kotlin
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

    fun reverseKGroup(head: ListNode?, k: Int): ListNode? {
        val newHead = ListNode(0).apply {
            next = head
        }
        var current = newHead

        while (canReverse(current, k)) {
            current = reverseNodes(current, k) ?: break
        }

        return newHead.next
    }

    private fun canReverse(prevHead: ListNode, k: Int): Boolean {
        var tailNode = prevHead
        for (i in 0 until k) {
            tailNode = tailNode.next ?: return false
        }
        return true
    }

    private fun reverseNodes(prevHead: ListNode?, k: Int): ListNode? {
        var currentNode: ListNode?
        for (i in (k - 1) downTo 0) {
            currentNode = prevHead
            for (j in 0 until i) {
                reverseSingleNode(currentNode)
                currentNode = currentNode?.next
            }
        }
        var tailNode = prevHead
        for( i in 0 until k) tailNode = tailNode?.next

        return tailNode
    }

    private fun reverseSingleNode(prevHead: ListNode?) {
        val targetNode = prevHead?.next
        val nextNode = targetNode?.next
        val tailNode = nextNode?.next

        prevHead?.next = nextNode
        nextNode?.next = targetNode
        targetNode?.next = tailNode
    }
}
```

### Explanation

k사이즈 만큼 앞에 노드가 존재하는지 확인 한 후, 버블 소트형태로 매 노트를 전환해주면 됨.
