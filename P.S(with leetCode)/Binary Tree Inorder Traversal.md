### Binary Tree Inorder Traversal



Today's Problem is Medium - [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

### Problem


Given a binary tree, return the inorder traversal of its nodes' values.

Example:

```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,3,2]
```

Follow up: Recursive solution is trivial, could you do it iteratively?


### Solution

```
import java.util.*

class Solution {
    fun inorderTraversal(root: TreeNode?): List<Int> {
        val stack: Deque<TreeNode?> = LinkedList()
        val result = mutableListOf<Int>()
        var curr = root
        while (curr != null || stack.isNotEmpty()) {
            while (curr != null) {
                stack.push(curr)
                curr = curr.left
            }
            curr = stack.pop()
            result.add(curr?.`val` ?: 0)

            curr = curr?.right
        }

        return result
    }
}
```

### Explanation

save the monkey-2 에서 데미안이 문제 풀었던 inorder travasal 의 반복문 해결방법으로해결!