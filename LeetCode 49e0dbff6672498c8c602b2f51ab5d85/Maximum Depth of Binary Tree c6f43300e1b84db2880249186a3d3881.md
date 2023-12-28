# Maximum Depth of Binary Tree

#: 104
Difficult: Easy
Tags: DFS, Divide and Conquer, Tree, toNull
link: https://leetcode.com/problems/maximum-depth-of-binary-tree/
Priority: Pass
Created time: July 8, 2022 4:26 PM
Last edited time: October 12, 2023 2:57 PM
practicedTimes: 1
source: HuaHua, LC_75, LC_Top_Interview_150, jiuzhang, 算初Tree Divide&Conquer

Given the `root` of a binary tree, return *its maximum depth*.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg](https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: 3

```

**Example 2:**

```
Input: root = [1,null,2]
Output: 2

```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `100 <= Node.val <= 100`

```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(!root) return 0;
        return max(maxDepth(root->left),maxDepth(root->right))+1;
    }
};
```