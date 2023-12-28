# Binary Tree Postorder Traversal

#: 145
Difficult: Easy
Tags: Iterate, Tree
link: https://leetcode.com/problems/binary-tree-postorder-traversal/
Priority: Low
Created time: July 8, 2022 4:26 PM
Last edited time: October 12, 2023 3:41 PM
practicedTimes: 1
source: HuaHua, jiuzhang, 算初Tree Divide&Conquer

Given the `root` of a binary tree, return *the postorder traversal of its nodes' values*.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/08/28/pre1.jpg](https://assets.leetcode.com/uploads/2020/08/28/pre1.jpg)

```
Input: root = [1,null,2,3]
Output: [3,2,1]

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

- The number of the nodes in the tree is in the range `[0, 100]`.
- `100 <= Node.val <= 100`

**Follow up:**

Recursive solution is trivial, could you do it iteratively?

Reverse preorder-mirror

```cpp
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        if(!root){return {};}
        vector<int> res;
        stack<TreeNode*> stk;
        stk.push(root);
        while(!stk.empty()){
            TreeNode* cur=stk.top(); stk.pop();
            res.push_back(cur->val);
            if(cur->left){stk.push(cur->left);}
            if(cur->right){stk.push(cur->right);}
        }
        reverse(res.begin(),res.end());
        return res;
    }
};
```