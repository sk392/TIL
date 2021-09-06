### Convert Sorted Array to Binary Search Tree


Today's Problem is Easy - [Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

### Problem

Given an integer array nums where the elements are sorted in ascending order, convert it to a height-balanced binary search tree.

A height-balanced binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.

![](https://assets.leetcode.com/uploads/2021/02/18/btree1.jpg)

```
Input: nums = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
Explanation: [0,-10,5,null,-3,null,9] is also accepted:

```
![](https://assets.leetcode.com/uploads/2021/02/18/btree2.jpg)


### Solution

```kotlin

class Solution {
    fun sortedArrayToBST(nums: IntArray): TreeNode? {
        return buildTree(nums, 0, nums.size-1)
    }

    fun buildTree(nums: IntArray, left: Int, right: Int): TreeNode? {
        if(left > right) return null
        val middleIndex = if(right-left==0) left else ((right-left)/2)+left
        val node = TreeNode(nums[middleIndex])
        node.left = buildTree(nums, left, middleIndex-1)
        node.right = buildTree(nums, middleIndex+1, right)
        return node
    }
}
```

### Explanation

BST만드는 것자체는 어렵지 않았으나, 재귀로 만들수 있도록 짜는게 중요했던듯! 오랜만에 워밍업좀 했다! brute force방법으로 진행했을때 Array를 넘겨주도록 했는데, 굳이 그럴 필요 없이 left, right한계를 지정하면 코드도 깔끔해지고 속도도 빨라진다!