### Flatten Binary Tree to Linked List


Today's Problem is Medium - [Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)

### Problem
Given a binary tree, flatten it to a linked list in-place.

For example, given the following tree:

```
    1
   / \
  2   5
 / \   \
3   4   6
```

The flattened tree should look like:

```
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
```


### Solution

```kotlin

class Solution {

    fun flatten(root: TreeNode?): Unit {
        if(root!= null)
        recursive(root)
    }

    private fun recursive(root: TreeNode){
        val left = root.left
        val right = root.right
        if(left != null ){
            recursive(left)
        }
        if(right != null ){
            recursive(right)
        }
        var leftLastNode = left

        while(leftLastNode?.right != null){
            leftLastNode = leftLastNode.right
        }


        if(left != null){
            root.right =left
            leftLastNode?.right = right
            root.left =null
        }
    }
}
```

```kotlin
//from discuss

fun flatten(root: TreeNode?, right: TreeNode? = null): Unit {
	root ?: return

	flatten(root.left, root.right ?: right)
	flatten(root.right, right)
	root.right = root.left ?: root.right ?: right
	root.left = null
}

```

### Explanation

후위 순회 방식으로 노드들을 돌면서, 오른쪽으로 정렬해주면 된다! discuss에서 괜찮은 솔루션을 봐서 함께 공유한다. 기존에 주어진 함수인 flatten을 변형해서 사용할 수 있다는 점도 인지하면 좋다!