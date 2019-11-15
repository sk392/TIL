### Remove Nth Node From End of List


오늘의 문제는 MEDIUM- [All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)

### Problem

Given a linked list, remove the n-th node from the end of list and return its head.

Example:

```
Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.
```

Note:

```
Given n will always be valid.
```


### Solution

```
import java.util.*

/**
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int = 0) {
 *     var next: ListNode? = null
 * }
 */
class Solution {
    fun removeNthFromEnd(head: ListNode?, n: Int): ListNode? {
         head?.next ?: return null
       val queue = ArrayDeque<ListNode?>()

        var result = head
        while (result != null) {
            addQueue(queue, result, n + 1)
            result = result.next
        }


        return if (queue.size == n + 1) {

            val tempNode = queue.pop()
            val nextNode = tempNode?.next?.next
            tempNode?.next = nextNode
            head
        } else {
            head.next
        }
    }

    fun addQueue(queue: Queue<ListNode?>, item: ListNode?, maxSize: Int) {
        if (queue.size >= maxSize) {
            queue.remove()
        }
        queue.add(item)
    }
}
```

### Explanation

이 문제의 해결방법은 크게 2가지, 첫 번째는 한바뀌 돌면서 각 노드에 인덱싱한 후에 두 번째 바퀴에서 제거하거나 두 번째는 첫번째 바퀴를 다 돌면서 노드를 다 저장한 후에 뒤에서 n번째 있는 노드를 제거하는 방식.

 문제를 조금 더 고민해보고 2번째 방법으로 풆이를 하는데, 저장공간을 조금 더 적게 쓰는 방법이 없을 까 했는데 그냥 큐에 넣고 N+1만큼의 공간만 있으면될 것같아서 문제를 해결, next가 없는 경우와 제일 앞에 노드를 제거하는 코드는 예외 처리로 들어갔고 나머진 같은 룰안에서 해결 댑니당