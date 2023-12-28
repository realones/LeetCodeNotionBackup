# Minimum Distance Between BST Nodes

#: 783
Difficult: Easy
Tags: Iterate, Tree
link: https://leetcode.com/problems/minimum-distance-between-bst-nodes/
Priority: Pass
Created time: February 17, 2023 12:41 AM
Last edited time: October 12, 2023 2:56 PM

Given the `root` of a Binary Search Tree (BST), return *the minimum difference between the values of any two different nodes in the tree*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/02/05/bst1.jpg](https://assets.leetcode.com/uploads/2021/02/05/bst1.jpg)

```
Input: root = [4,2,6,1,3]
Output: 1

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/02/05/bst2.jpg](https://assets.leetcode.com/uploads/2021/02/05/bst2.jpg)

```
Input: root = [1,0,48,null,null,12,49]
Output: 1

```

**Constraints:**

- The number of nodes in the tree is in the range `[2, 100]`.
- `0 <= Node.val <= 105`

**Note:** This question is the same as 530: [https://leetcode.com/problems/minimum-absolute-difference-in-bst/](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)

```jsx
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
    int minDiffInBST(TreeNode* root) {
        stack<TreeNode*> stk;
        int res=INT_MAX;
        int lastVal=INT_MAX;
        pushL(stk,root);
        while(!stk.empty()){
            TreeNode* cur=stk.top(); stk.pop();
            int diff=(lastVal!=INT_MAX)?abs(cur->val - lastVal):INT_MAX;
            res=min(res,diff);
            lastVal=cur->val;
            if(cur->right){pushL(stk,cur->right);}
        }
        return res;
    }
    
    void pushL(stack<TreeNode*> &stk, TreeNode* node){
        while(node){
            stk.push(node);
            node=node->left;
        }
    }
};
```