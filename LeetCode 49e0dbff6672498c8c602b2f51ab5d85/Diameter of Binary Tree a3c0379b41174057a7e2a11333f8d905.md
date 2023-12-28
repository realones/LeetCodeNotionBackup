# Diameter of Binary Tree

#: 543
Difficult: Easy
Tags: DFS, Tree
link: https://leetcode.com/problems/diameter-of-binary-tree/description/
Priority: Low
Created time: June 27, 2023 11:51 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Binary Tree Maximum Path Sum (Binary%20Tree%20Maximum%20Path%20Sum%20f544ed88691540c5af96eba3bb262c14.md), Diameter of Binary Tree (Diameter%20of%20Binary%20Tree%20f378c6013a184759bf96c2149bbdfb93.md)

Given the `root` of a binary tree, return *the length of the **diameter** of the tree*.

The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the `root`.

The **length** of a path between two nodes is represented by the number of edges between them.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)

```
Input: root = [1,2,3,4,5]
Output: 3
Explanation: 3 is the length of the path [4,2,1,3] or [5,2,1,3].

```

**Example 2:**

```
Input: root = [1,2]
Output: 1

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `100 <= Node.val <= 100`

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int res=0;

    int diameterOfBinaryTree(TreeNode* root) {
        if(!root) return 0;
        helper(root);
        return res;
    }

    //longest path of one side from root
    int helper(TreeNode* root){
        if(!root) return -1;
        int l=helper(root->left);
        int r=helper(root->right);
        res=max(res,l+r+2);
        return max(l,r)+1;
    }
};

/*
return: length of the diameter of the tree
diameter: max number of edges between two nodes
*
```