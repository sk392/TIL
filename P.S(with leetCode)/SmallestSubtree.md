### Smallest Subtree with all the Deepest Nodes


요번엔 [Smallest Subtree with all the Deepest Nodes](https://leetcode.com/problems/smallest-subtree-with-all-the-deepest-nodes/)요거를 풀어봐씁니다.

### Problem
Given a binary tree rooted at root, the depth of each node is the shortest distance to the root.

A node is deepest if it has the largest depth possible among any node in the entire tree.

The subtree of a node is that node, plus the set of all descendants of that node.

Return the node with the largest depth such that it contains all the deepest nodes in its subtree.

 
```
Example 1:

Input: [3,5,1,6,2,0,8,null,null,7,4]
Output: [2,7,4]
Explanation:



We return the node with value 2, colored in yellow in the diagram.
The nodes colored in blue are the deepest nodes of the tree.
The input "[3, 5, 1, 6, 2, 0, 8, null, null, 7, 4]" is a serialization of the given tree.
The output "[2, 7, 4]" is a serialization of the subtree rooted at the node with value 2.
Both the input and output have TreeNode type.
 
```
![이미지](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/01/sketch1.png)

```
Note:

The number of nodes in the tree will be between 1 and 500.
The values of each node are unique.
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
    fun subtreeWithAllDeepest(root: TreeNode?): TreeNode? {

        val left = depth(root?.left)
        val right = depth(root?.right)
        return when {
            left == right -> root
            left > right -> subtreeWithAllDeepest(root?.left)
            else -> subtreeWithAllDeepest(root?.right)
        }
    }

    fun depth(root : TreeNode?) : Int {
        if(root == null) return 0
        return Math.max(depth(root.left),depth(root.right)) +1
    }
}


```

### Explanation

문제풀다가 실패..해서 Discuss에서 본 솔루션은 내가 생각만 했던 로직중에 하나였다.
 무의식중에 오래걸리는 코드는 안되! 라는 생각 때문에 작성하지 않았는데, 실제론 속도 100%로 가장 빠른 로직이 되었다.. 이런 무의식은 조심하는 것이 좋겠다.