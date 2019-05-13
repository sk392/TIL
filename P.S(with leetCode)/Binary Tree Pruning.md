### Binary Tree Pruning

요번엔 [Binary Tree Pruning](https://leetcode.com/problems/binary-tree-pruning/)요거를 풀어봐씁니다.

### Problem
We are given the head node root of a binary tree, where additionally every node's value is either a 0 or a 1.

Return the same tree where every subtree (of the given tree) not containing a 1 has been removed.

(Recall that the subtree of a node X is X, plus every node that is a descendant of X.)

```
Example 1:
Input: [1,null,0,0,1]
Output: [1,null,0,null,1]
 
Explanation: 
Only the red nodes satisfy the property "every subtree not containing a 1".
The diagram on the right represents the answer.
```
![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/06/1028_2.png)

```
Example 2:
Input: [1,0,1,0,0,0,1]
Output: [1,null,1,null,1]
```

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/06/1028_1.png)

```
Example 3:
Input: [1,1,0,1,1,0,1,0]
Output: [1,1,0,1,1,null,1]
```

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/05/1028.png)


```
Note:

The binary tree will have at most 100 nodes.
The value of each node will only be 0 or 1.
```

### Solution

```
/**
 * Example:
 * var ti = TreeNode(5)
 * var v = ti.`val`
 * Definition for a binary tree node.
 * class TreeNode(var `val`: Int) {
 *     var left: TreeNode? = null
 *     var right: TreeNode? = null
 * }
 */
class Solution {
    fun pruneTree(root: TreeNode?): TreeNode? {
        pruning(root)
        return root
    }

    private fun pruning(root: TreeNode?): Boolean {
        root ?: return true

        val isPruningLeft = pruning(root.left)
        val isPruningRight = pruning(root.right)
        if (isPruningLeft && isPruningRight && root.`val` == 0) {
            root.left = null
            root.right = null
            return true
        }

        if (isPruningLeft) root.left = null
        if (isPruningRight) root.right = null
        return false
    }
}
```

### Explanation

올ㅋ 한번에 풀었다. 가장끝에 노드부터 죽은 가지인지 확인하면서 올라와야 해서 DFS로 풀어야겠다는 생각을 했고 재귀로 해결!