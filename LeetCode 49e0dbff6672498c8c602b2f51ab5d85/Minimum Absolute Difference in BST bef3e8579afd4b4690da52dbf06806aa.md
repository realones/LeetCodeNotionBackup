# Minimum Absolute Difference in BST

#: 530
Difficult: Easy
Tags: Tree
link: https://leetcode.com/problems/minimum-absolute-difference-in-bst/
Priority: Pass
Created time: June 5, 2023 1:11 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_Top_Interview_150

Given the `root` of a Binary Search Tree (BST), return *the minimum absolute difference between the values of any two different nodes in the tree*.

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

- The number of nodes in the tree is in the range `[2, 104]`.
- `0 <= Node.val <= 105`

**Note:** This question is the same as 783: [https://leetcode.com/problems/minimum-distance-between-bst-nodes/](https://leetcode.com/problems/minimum-distance-between-bst-nodes/)

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
    int getMinimumDifference(TreeNode* root) {
        if(!root){return -1;}
        int last=-1e5, res=1e5;
        stack<TreeNode*> stk;
        pushLeft(stk,root);
        while(!stk.empty()){
            TreeNode* cur=stk.top(); stk.pop();
            res=min(res, cur->val-last);
            last=cur->val;
            if(cur->right){pushLeft(stk,cur->right);}
        }
        return res;
    }

    void pushLeft(stack<TreeNode*>& stk, TreeNode* node){
        while(node){
            stk.push(node);
            node=node->left;
        }
    }
};
```