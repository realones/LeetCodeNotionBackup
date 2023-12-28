# Convert BST to Greater Tree

#: 538
Difficult: Medium
Tags: Iterate, Tree
link: https://leetcode.com/problems/convert-bst-to-greater-tree/
Priority: Pass
Created time: October 17, 2022 6:58 PM
Last edited time: October 12, 2023 2:56 PM
source: googleFreq

Given the `root` of a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus the sum of all keys greater than the original key in BST.

As a reminder, a *binary search tree* is a tree that satisfies these constraints:

- The left subtree of a node contains only nodes with keys **less than** the node's key.
- The right subtree of a node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/05/02/tree.png](https://assets.leetcode.com/uploads/2019/05/02/tree.png)

```
Input: root = [4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
Output: [30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]

```

**Example 2:**

```
Input: root = [0,null,1]
Output: [1,null,1]

```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `104 <= Node.val <= 104`
- All the values in the tree are **unique**.
- `root` is guaranteed to be a valid binary search tree.

**Note:** This question is the same as 1038: [https://leetcode.com/problems/binary-search-tree-to-greater-sum-tree/](https://leetcode.com/problems/binary-search-tree-to-greater-sum-tree/)

solution: reverse in order iterator

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
    TreeNode* convertBST(TreeNode* root) {
        if(!root) return NULL;
        int last=0;
        stack<TreeNode*> stk;
        pushRight(stk, root);
        while(!stk.empty()){
            TreeNode* cur=stk.top(); stk.pop();
            cur->val+=last;
            last=cur->val;
            pushRight(stk,cur->left);
        }
        return root;
    }
    
    void pushRight(stack<TreeNode*>& stk, TreeNode* node){
        while(node){
            stk.push(node);
            node=node->right;
        }
    }
};
```