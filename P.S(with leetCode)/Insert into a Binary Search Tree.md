### Insert into a Binary Search Tree


요번엔 [Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/)요거를 풀어봐씁니다.

### Problem

Given the root node of a binary search tree (BST) and a value to be inserted into the tree, insert the value into the BST. Return the root node of the BST after the insertion. It is guaranteed that the new value does not exist in the original BST.

Note that there may exist multiple valid ways for the insertion, as long as the tree remains a BST after insertion. You can return any of them.

For example, 

```
Given the tree:
        4
       / \
      2   7
     / \
    1   3
And the value to insert: 5
```

You can return this binary search tree:


```
         4
       /   \
      2     7
     / \   /
    1   3 5
```

This tree is also valid:

```
         5
       /   \
      2     7
     / \   
    1   3
         \
          4
          
          
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
    fun insertIntoBST(root: TreeNode?, `val`: Int): TreeNode? {
        insert(root,`val`)
        return root
    }

    fun insert(root :TreeNode?, `val` : Int){
        root?: return
        when{
            root.`val` < `val` && root.right == null  -> {
                root.right = TreeNode(`val`)
            }
            root.`val` > `val`&& root.left == null -> {
                root.left = TreeNode(`val`)
            }
            root.`val` < `val` -> {
                insert(root.right,`val`)
            }
            root.`val` > `val` -> {
                insert(root.left,`val`)
            }
        }
    }
}

```

### Explanation

여러번 풀었던 유형은 한번에 풀리는 경우도 많은 것같다! 단 정답률 60퍼넘는 문제만..ㅋㅋㅋㅋ BST특징만 이해하고 있으면 바로풀 수 있는 문제!
