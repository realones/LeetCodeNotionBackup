# Univalued Binary Tree

#: 965
Difficult: Easy
Tags: BFS, DFS, Iterate, Tree
link: https://leetcode.com/problems/univalued-binary-tree/description/
Priority: Pass
Created time: August 12, 2023 4:56 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

A binary tree is **uni-valued** if every node in the tree has the same value.

Given the `root` of a binary tree, return `true` *if the given tree is **uni-valued**, or* `false` *otherwise.*

**Example 1:**

![https://assets.leetcode.com/uploads/2018/12/28/unival_bst_1.png](https://assets.leetcode.com/uploads/2018/12/28/unival_bst_1.png)

```
Input: root = [1,1,1,1,1,null,1]
Output: true

```

**Example 2:**

![https://assets.leetcode.com/uploads/2018/12/28/unival_bst_2.png](https://assets.leetcode.com/uploads/2018/12/28/unival_bst_2.png)

```
Input: root = [2,2,2,5,2]
Output: false

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 100]`.
- `0 <= Node.val < 100`

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
    bool isUnivalTree(TreeNode* root) {
        if(!root) return true;
        int val=root->val;
        stack<TreeNode*> stk;
        stk.push(root);
        while(!stk.empty()){
            TreeNode* cur=stk.top();stk.pop();
            if(cur->val != val) return false;
            if(cur->right) stk.push(cur->right);
            if(cur->left) stk.push(cur->left);
        }
        return true;
    }
}
```