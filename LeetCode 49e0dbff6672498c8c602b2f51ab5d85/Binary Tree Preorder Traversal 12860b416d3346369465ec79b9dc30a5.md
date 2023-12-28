# Binary Tree Preorder Traversal

#: 144
Difficult: Easy
Tags: Iterate, Tree
link: https://leetcode.com/problems/binary-tree-preorder-traversal/
Priority: Low
Created time: July 8, 2022 4:26 PM
Last edited time: October 12, 2023 3:41 PM
practicedTimes: 1
source: HuaHua, jiuzhang, 算初Tree Divide&Conquer

Given the `root` of a binary tree, return *the preorder traversal of its nodes' values*.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg](https://assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg)

```
Input: root = [1,null,2,3]
Output: [1,2,3]

```

**Example 2:**

```
Input: root = []
Output: []

```

**Example 3:**

```
Input: root = [1]
Output: [1]

```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 100]`.
- `100 <= Node.val <= 100`

**Follow up:** Recursive solution is trivial, could you do it iteratively?

```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        if(!root) return {};
        vector<int> res;
        stack<TreeNode*> stk;
        stk.push(root);
        while(!stk.empty()){
            TreeNode* cur=stk.top(); stk.pop();
            res.push_back(cur->val);
            if(cur->right) stk.push(cur->right);
            if(cur->left) stk.push(cur->left);
        }
        return res;
    }
};
```