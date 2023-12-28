# Balanced Binary Tree

#: 110
Difficult: Easy
Tags: Divide and Conquer, Tree, toNull
link: https://leetcode.com/problems/balanced-binary-tree
Priority: Low
Created time: July 8, 2022 4:26 PM
Last edited time: October 12, 2023 3:41 PM
practicedTimes: 1
source: HuaHua, jiuzhang, 算初Tree Divide&Conquer

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

> a binary tree in which the left and right subtrees of every node differ in height by no more than 1.
> 

**Example 1:**

![https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg](https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: true

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/10/06/balance_2.jpg](https://assets.leetcode.com/uploads/2020/10/06/balance_2.jpg)

```
Input: root = [1,2,2,3,3,null,null,4,4]
Output: false

```

**Example 3:**

```
Input: root = []
Output: true

```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 5000]`.
- `104 <= Node.val <= 104`

```cpp
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        if(depth(root)==-1) return false;
        return true;
    }
    int depth(TreeNode* node){
        if(node==NULL) return 0;
        int ld=depth(node->left);
        if(ld==-1) return -1;
        int rd=depth(node->right);
        if(rd==-1) return -1;
        if(abs(ld-rd)<=1) return max(ld,rd)+1;
        else return -1;
    }
};
```