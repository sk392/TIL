###  Increasing Order Search Tree


요번엔 [Increasing Order Search Tree](https://leetcode.com/problems/increasing-order-search-tree/)요거를 풀어봐씁니다.

### Problem
Given a tree, rearrange the tree in in-order so that the leftmost node in the tree is now the root of the tree, and every node has no left child and only 1 right child.

```
Example 1:
Input: [5,3,6,2,4,null,8,1,null,null,null,7,9]

       5
      / \
    3    6
   / \    \
  2   4    8
 /        / \
1        7   9

Output: [1,null,2,null,3,null,4,null,5,null,6,null,7,null,8,null,9]

 1
  \
   2
    \
     3
      \
       4
        \
         5
          \
           6
            \
             7
              \
               8
                \
                 9
                 
```

```
Note:

The number of nodes in the given tree will be between 1 and 100.
Each node will have a unique integer value from 0 to 1000.
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

    val increasingBSTResult = TreeNode(0)
    var node = increasingBSTResult
    
    fun increasingBST(root: TreeNode?): TreeNode? {
        addQueue(root)
        return increasingBSTResult.right
    }

    fun addQueue(root: TreeNode?) {
        root ?: return
        addQueue(root.left)
        node.right = TreeNode(root.`val`)
        node = node.right!!
        addQueue(root.right)

    }
}
```

### Explanation

그냥 정렬된 숫자 배열만 있으면 된다고 생각해서 트리를 모두 돌면서 큐에 넣는 작업을하고 나중에 for문으로 루프돌려서 만드는 방법을쓰다가 큐에 넣지 않고 바로 노드를 만들면되겠다 싶어서 수정함!