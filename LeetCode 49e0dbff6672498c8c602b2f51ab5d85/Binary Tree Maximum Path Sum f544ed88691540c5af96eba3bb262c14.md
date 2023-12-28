# Binary Tree Maximum Path Sum

#: 124
Difficult: Hard
Tags: Divide and Conquer, Tree
link: https://leetcode.com/problems/binary-tree-maximum-path-sum/
Priority: Medium
Created time: July 16, 2022 11:50 AM
Last edited time: November 17, 2023 1:27 AM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, jiuzhang, ticktokFreq, 算初Tree Divide&Conquer
related: Diameter of Binary Tree (Diameter%20of%20Binary%20Tree%20a3c0379b41174057a7e2a11333f8d905.md), Diameter of Binary Tree (Diameter%20of%20Binary%20Tree%20f378c6013a184759bf96c2149bbdfb93.md)

A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence **at most once**. Note that the path does not need to pass through the root.

The **path sum** of a path is the sum of the node's values in the path.

Given the `root` of a binary tree, return *the maximum **path sum** of any **non-empty** path*.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/10/13/exx1.jpg](https://assets.leetcode.com/uploads/2020/10/13/exx1.jpg)

```
Input: root = [1,2,3]
Output: 6
Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/10/13/exx2.jpg](https://assets.leetcode.com/uploads/2020/10/13/exx2.jpg)

```
Input: root = [-10,9,20,null,null,15,7]
Output: 42
Explanation: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 3 * 104]`.
- `1000 <= Node.val <= 1000`

maxPath一个method做两件事情，不美

```cpp
class Solution {
public:
    int res = INT_MIN;
    int maxPathSum(TreeNode* root) {
        if(!root) return res;
        maxPath(root);
        return res;
    }
    
    int maxPath(TreeNode* node){
        if(!node) return 0;
        int l = maxPath(node->left);
        int r = maxPath(node->right);
        res=max(res, node->val + max(l,0) + max(r,0));
        return node->val + max({l,r,0});
    }
};
```