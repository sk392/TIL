### Validate Binary Search Tree

Today's Problem is Medium - [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/) 

### Problem

Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.
 

Example 1:

```
    2
   / \
  1   3

Input: [2,1,3]
Output: true
```

```
Example 2:

    5
   / \
  1   4
     / \
    3   6

Input: [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```
   
### Solution

```
import java.util.*
class Solution {
    fun isValidBST(root: TreeNode?): Boolean {
        var prevVal:Int?= null
        var curr =root
        val stack : Deque<TreeNode?> = LinkedList()

        while(curr != null || stack.isNotEmpty()){
            while(curr != null){
                stack.push(curr)
                curr = curr.left
            }
            curr = stack.pop()!!

            if(prevVal != null && prevVal >=curr.`val` ) return false

            prevVal = curr.`val`
            curr = curr.right
        }

        return true
    }
}
```

### Explanation

InOrder Traversal을 하다보니 계속 비슷한 문제가 나오네.. ! stack방법에 익숙해지기 위해서 역시 같은 방법을 사용했다. 

이전 값을 계속 기억해뒀다가 increse되지 않는 구간에서 바로 return 처리해서 해결!