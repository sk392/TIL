### Swap Nodes in Pairs


Today's Problem is Medium - [Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

### Problem


Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

 

Example 1:

![](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)

```
Input: head = [1,2,3,4]
Output: [2,1,4,3]
```

Example 2:

```
Input: head = []
Output: []
```

Example 3:

```
Input: head = [1]
Output: [1]
```

Constraints:

`The number of nodes in the list is in the range [0, 100].`

`0 <= Node.val <= 100`

### Solution

```
class Solution {
    fun swapPairs(head: ListNode?): ListNode? {
        var newHead = ListNode(0)
        newHead.next = head
        var latestNode = newHead
        var firstNode = latestNode.next
        var secondNode = firstNode?.next
        
        while (firstNode != null && secondNode != null) {
            swapPair(latestNode, firstNode, secondNode)
            latestNode = firstNode
            firstNode = firstNode.next
            secondNode = firstNode?.next
        }

        return newHead.next
    }

    private fun swapPair(latestNode: ListNode, firstNode: ListNode, secondNode: ListNode) {
        latestNode.next = secondNode
        firstNode.next = secondNode.next
        secondNode.next = firstNode
    }
}
```

### Explanation

기본적으로 bubbleSort와 비슷한 방법으로 해결했다. Pair 단위로 업데이트를 하기 위해서는 총 3가지의 노드를 기억해야하는데, 변경할 첫번째를 기억중인 노드와 실제로 바꿀 두 노드가 필요하다. 재귀를 통해 코드를 줄일 수도 있었지만, 순차반복문을 통해 메모리를 많이 먹지 않게 해결했다.
시간복잡도는 O(n)이고 공간 복잡도는 O(1)이다.