# Largest BST Subtree

#: 333
Difficult: Medium
Tags: BST, DFS, DP, Tree
link: https://leetcode.com/problems/largest-bst-subtree/description/
Priority: Low
Status: More Solution
Created time: December 6, 2023 3:57 AM
Last edited time: December 6, 2023 3:59 AM
source: ticktokFreq

Given the root of a binary tree, find the largest

subtree

, which is also a Binary Search Tree (BST), where the largest means subtree has the largest number of nodes.

A **Binary Search Tree (BST)** is a tree in which all the nodes follow the below-mentioned properties:

- The left subtree values are less than the value of their parent (root) node's value.
- The right subtree values are greater than the value of their parent (root) node's value.

**Note:** A subtree must include all of its descendants.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/10/17/tmp.jpg](https://assets.leetcode.com/uploads/2020/10/17/tmp.jpg)

```
Input: root = [10,5,15,1,8,null,7]
Output: 3
Explanation:The Largest BST Subtree in this case is the highlighted one. The return value is the subtree's size, which is 3.
```

**Example 2:**

```
Input: root = [4,2,7,2,3,5,null,2,null,null,null,null,null,1]
Output: 2

```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `104 <= Node.val <= 104`

**Follow up:** Can you figure out ways to solve it with `O(n)` time complexity?

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
    int largestBSTSubtree(TreeNode* root) {
        if(!root) return 0;
        if (isBST(root,INT_MIN, INT_MAX)){ return count(root); } 
        return max(largestBSTSubtree(root->left),largestBSTSubtree(root->right));
    }
private:
    bool isBST(TreeNode* root, int min, int max){
        if(!root) return true;
        if(root->val <= min || root->val >= max) return false;        
        return isBST(root->left,min,root->val) && isBST(root->right,root->val,max);   
    }
    int count(TreeNode* root){
        if(!root) return 0;
        return 1+count(root->left)+count(root->right);
    }
};

//from ans to save time
```