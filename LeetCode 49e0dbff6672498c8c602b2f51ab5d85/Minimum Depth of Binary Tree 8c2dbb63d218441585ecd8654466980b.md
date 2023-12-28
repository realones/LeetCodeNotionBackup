# Minimum Depth of Binary Tree

#: 111
Difficult: Easy
Tags: Divide and Conquer, Tree, toLeaf
link: https://leetcode.com/problems/minimum-depth-of-binary-tree
Priority: Low
Created time: July 8, 2022 4:26 PM
Last edited time: October 13, 2023 12:47 PM
practicedTimes: 1
source: HuaHua, jiuzhang, 算初Tree Divide&Conquer

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note:** A leaf is a node with no children.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/10/12/ex_depth.jpg](https://assets.leetcode.com/uploads/2020/10/12/ex_depth.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: 2

```

**Example 2:**

```
Input: root = [2,null,3,null,4,null,5,null,6]
Output: 5

```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 105]`.
- `1000 <= Node.val <= 1000`

```cpp
class Solution {
public:
    int minDepth(TreeNode* root) {
        if(!root) return 0;
        if(!root->left&&!root->right) return 1;
        if(!root->left) return minDepth(root->right)+1;
        if(!root->right) return minDepth(root->left)+1;
        return min(minDepth(root->left), minDepth(root->right))+1;
    }
};
```