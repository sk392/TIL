### SymmetricTree


요번엔 [SymmetricTree](https://leetcode.com/problems/symmetric-tree/)요거를 풀어봐씁니다.

### Problem


Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree [1,2,2,3,4,4,3] is symmetric:

```
    1
   / \
  2   2
 / \ / \
3  4 4  3

```


But the following [1,2,2,null,3,null,3] is not:

```
    1
   / \
  2   2
   \   \
   3    3
 ```
 
### Solution

```
class Solution {
    fun isSymmetric(root: TreeNode?): Boolean {
        if(root == null) return true
        return recursion(root.left,root.right)
    }

    fun recursion(left: TreeNode?, right: TreeNode?): Boolean {
        return when {
            left == null && right == null -> true
            left == null || right == null -> false
            else -> (left.`val` == right.`val`) && recursion(left.left,right.right) && recursion(left.right,right.left)
        }
    }
}
```

### Explanation

요 문제도 DFS(Depth First Search)로 해결해따! ㅋㅋㅋ 맨첨엔 순차 서치해서 해결했는데 DFS로 푸는게 훨씬 빠르고 코드도 짧다 굿