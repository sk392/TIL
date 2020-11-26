### Reverse Linked List



Today's Problem is Easy - [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

### Problem


Reverse a singly linked list.

Example:

```
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL
```


### Solution

```

class Solution {
    fun reverseList(head: ListNode?): ListNode? {
        val head=  head ?: return null
        val currentNode = head
        val nextNode = head.next ?: return currentNode
        val newHead = reverseList(nextNode)
        nextNode.next = currentNode
        currentNode.next = null
        return newHead
    }
}
```

### Explanation

가장 간단하게는 우선 map에 카운트 값 넣으면서 인덱싱을 해줘서 나중에 해당 카운트 값에서 뒤로 가면서 노드를 꺼내서 연결하면 된다. 
위에 방법에서 메모리를 덜 쓰는 방법은 현재 노드와 넥스트 노드를 저장하고 리커시브하게 돌린다.

 기본적인 룰은 현재노드와 다음 노드의 순서를 바꾸면 되는 일인데, 그게 끝에서 부터 이루어져야 한다는 점이다! 그걸 유의해서 리커시브 부터 돌리고 그 다음에 현재 노드의 순서를 바꿔준다.
 
 마지막으로 currentNode.next에 null을 넣어주지 않으면 사이클이 생기니 유의하자!