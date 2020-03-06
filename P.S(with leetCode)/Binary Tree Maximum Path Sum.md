### Binary Tree Maximum Path Sum



Today's Problem is Hard - [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-inorder-traversal/)

### Problem

Given a non-empty binary tree, find the maximum path sum.

For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain at least one node and does not need to go through the root.

Example 1:

```
Input: [1,2,3]

       1
      / \
     2   3

Output: 6
```

Example 2:

```
Input: [-10,9,20,null,null,15,7]

   -10
   / \
  9  20
    /  \
   15   7

Output: 42
```

### Solution

```
class Solution {
    var maxPathSumResult = Int.MIN_VALUE
    fun maxPathSum(root: TreeNode?): Int {
        recursive(root!!)
        return maxPathSumResult
    }

    private fun recursive(root: TreeNode): Int {
        val leftChild = root.left
        val rightChild = root.right
        var result = root.`val`

        when {
            leftChild != null && rightChild != null -> {
                val leftMaxValue = recursive(leftChild)
                val rightMaxValue = recursive(rightChild)

                maxPathSumResult = Math.max(maxPathSumResult, root.`val` + leftMaxValue + rightMaxValue)
                result = Math.max(
                    result,
                    Math.max(leftMaxValue, rightMaxValue) + root.`val`
                )
            }
            leftChild != null -> {
                result = Math.max(
                    result,
                    recursive(leftChild) + root.`val`
                )
            }
            rightChild != null -> {
                result = Math.max(
                    result,
                    recursive(rightChild) + root.`val`
                )
            }
        }

        maxPathSumResult = Math.max(maxPathSumResult, result)
        return result
    }
```

### Explanation

처음엔 패쓰를 구하는 문제라고 그래서 루트부터 리프노드까지의 길이에서 최대값을 찾는 줄 알았는데, 루트를 꼭 포함할 필요는 없다. 또한 노드가 꼭 리프노드를 가져야할 필요는 없으며 최대 2개의 리프노드를 가질 수 있다는 것을 알 수 있다.(시작 리프노드 끝 리프노드)

솔루션의 개념은 하단의 노드부터 시작해서 현재노드까지의 최대합을 반환해주고 자식이 2명 모두 있는 경우면 루트까지 합해서 최대 값을 갱신해준다(현재 노드를 루트노드로 삼는셈) 그리고 반환할 때는 둘중 큰 값만 넘겨주게되는데, 이래야 하나의 선이 그어져 본인 상위 노드에서 양 노드의 합을 계산할 때 패스형태로 구할 수 있다.