###  Minimum Distance Between BST Nodes


Today's Problem is Easy - [ Minimum Distance Between BST Nodes](https://leetcode.com/problems/minimum-distance-between-bst-nodes/)

### Problem

Given a Binary Search Tree (BST) with the root node root, return the minimum difference between the values of any two different nodes in the tree.

Example :

```
Input: root = [4,2,6,1,3,null,null]
Output: 1
Explanation:
Note that root is a TreeNode object, not an array.

The given tree [4,2,6,1,3,null,null] is represented by the following diagram:

          4
        /   \
      2      6
     / \    
    1   3  

while the minimum difference in this tree is 1, it occurs between node 1 and node 2, also between node 3 and node 2.
```
Note:

1. The size of the BST will be between 2 and 100.
2. The BST is always valid, each node's value is an integer, and each node's value is different.


### Solution

```
import java.util.LinkedList
import java.util.Deque

class Solution {
    fun minDiffInBST(root: TreeNode?): Int {
        val stack: Deque<TreeNode> = LinkedList()
        var prev: TreeNode? = null
        var current = root
        var result = Int.MAX_VALUE

        while (current != null || stack.isNotEmpty()) {
            while (current != null) {
                stack.push(current)
                current = current.left
            }

            current = stack.pop()

            if (prev != null && current != null) {
                result = Math.min(result, (current.`val`) - prev.`val`)
            }
            prev = current
            current = current.right
        }

        return result
    }
}
```

### Explanation

save the mockey에서 푼 문젠데 재귀가 아니라 stack을 사용해서 리니어하게 푸는 방법으로 해결해봤다.

1. 자신과 자신 왼쪽자식을 쭉 스택에 넣는다.
2. stack에 맨 위에 값을 꺼내고 prev가 존재한다면 min을 갱신한다
3. prev에 현재를 넣어주고 current를 내 오른쪽 자식을 넣는다.
4. 위 과정을 반복한다.

한눈에 이해가 되지 않는 코드인데, 디버깅을 해야 알 수 있었다. 아름다운 로직이라고 생각한다.