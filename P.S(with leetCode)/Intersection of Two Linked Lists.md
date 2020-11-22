### Intersection of Two Linked Lists



요번엔 [Intersection of Two Linked Lists
](https://leetcode.com/problems/intersection-of-two-linked-lists/)요거를 풀어봐씁니다.

### Problem

GWrite a program to find the node at which the intersection of two singly linked lists begins.

For example, the following two linked lists:


begin to intersect at node c1.

![](https://assets.leetcode.com/uploads/2018/12/13/160_statement.png)


 

Example 1:

![](https://assets.leetcode.com/uploads/2020/06/29/160_example_1_1.png)

```
Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
Output: Reference of the node with value = 8
Input Explanation: The intersected node's value is 8 (note that this must not be 0 if the two lists intersect). From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,6,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.
```

### Solution

```
class Solution {
    fun getIntersectionNode(headA:ListNode?, headB:ListNode?):ListNode? {
        var pA = headA
        var pB = headB
        while (pA != pB) {
            if (pA?.next == null && pB?.next == null && pA != pB) return null
            pA = if (pA?.next == null) headB else pA.next
            pB = if (pB?.next == null) headA else pB.next
        }
        return pA
    }
}
```

### Explanation

오우.. 몇달만에 푸는 문제여서 그런지 이지도 쉽지 않았다. 
처음엔 한쪽 노드 리스트를 맵에 넣고 다른 리스트를 서치할 때 컨테인 체크로 문제를 해결헀다. 그 후 discuss에서 현재 코드를 찾았는데, A노드와 B노드를 각각 순차적으로 한번씩 순회하면 A + B , B + A의 길이가 같다는 점과 길이가 같다면 교차점으로부터 의 거리도 같으므로 같은 방법으로 순회하면 해결!