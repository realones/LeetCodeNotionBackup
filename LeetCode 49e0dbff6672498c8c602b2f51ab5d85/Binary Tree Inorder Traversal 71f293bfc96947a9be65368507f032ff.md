# Binary Tree Inorder Traversal

#: 94
Difficult: Easy
Tags: Iterate, Tree
link: https://leetcode.com/problems/binary-tree-inorder-traversal/
Priority: Low
Created time: July 8, 2022 4:26 PM
Last edited time: October 12, 2023 3:41 PM
practicedTimes: 1
source: HuaHua, jiuzhang, 算初Tree Divide&Conquer

Given the `root` of a binary tree, return *the inorder traversal of its nodes' values*.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg](https://assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg)

```
Input: root = [1,null,2,3]
Output: [1,3,2]

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

**Follow up:**

Recursive solution is trivial, could you do it iteratively?

注意pass by reference

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        if(!root){return {};}
        vector<int> res;
        stack<TreeNode*> stk;
        pushLeft(stk,root);
        while(!stk.empty()){
            TreeNode* cur=stk.top(); stk.pop();
            std::cout << cur->val << std::endl;
            res.push_back(cur->val);
            if(cur->right){pushLeft(stk,cur->right);}
        }
        return res;
    }

    void pushLeft(stack<TreeNode*> &stk, TreeNode* node){
        while(node){
            stk.push(node);
            node=node->left;
        }
    }
};
```