# Validate Binary Search Tree

#: 98
Difficult: Medium
Tags: Iterate, Tree
link: https://leetcode.com/problems/validate-binary-search-tree/
Priority: Low
Created time: July 8, 2022 4:26 PM
Last edited time: October 13, 2023 12:48 PM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, jiuzhang, 算初Tree Divide&Conquer

Given the `root` of a binary tree, *determine if it is a valid binary search tree (BST)*.

A **valid BST** is defined as follows:

- The left subtree of a node contains only nodes with keys **less than** the node's key.
- The right subtree of a node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/12/01/tree1.jpg](https://assets.leetcode.com/uploads/2020/12/01/tree1.jpg)

```
Input: root = [2,1,3]
Output: true

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/12/01/tree2.jpg](https://assets.leetcode.com/uploads/2020/12/01/tree2.jpg)

```
Input: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `231 <= Node.val <= 231 - 1`

```cpp
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        if(!root){return true;}
        long long preVal=(long long)INT_MIN-1;
        stack<TreeNode*> stk;
        pushLeft(root,stk);
        while(!stk.empty()){
            TreeNode* curNode=stk.top(); stk.pop();
            int curVal=curNode->val;
            if(preVal>=curVal){return false;}
            else{preVal=curVal;}
            pushLeft(curNode->right,stk);
        }
        return true;
    }

    void pushLeft(TreeNode* root, stack<TreeNode*>& stk){
        while(root){
            stk.push(root);
            root=root->left;
        }
    }
};
```