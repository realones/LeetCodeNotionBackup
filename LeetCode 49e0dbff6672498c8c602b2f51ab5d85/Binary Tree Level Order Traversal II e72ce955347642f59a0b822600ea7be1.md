# Binary Tree Level Order Traversal II

#: 107
Difficult: Medium
Tags: BFS, Tree
link: https://leetcode.com/problems/binary-tree-level-order-traversal-ii/
Priority: Pass
Created time: July 17, 2022 11:52 PM
Last edited time: October 13, 2023 1:01 PM
practicedTimes: 1
source: HuaHua, jiuzhang, 算初BFS

Given the `root` of a binary tree, return *the bottom-up level order traversal of its nodes' values*. (i.e., from left to right, level by level from leaf to root).

**Example 1:**

![https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: [[15,7],[9,20],[3]]

```

**Example 2:**

```
Input: root = [1]
Output: [[1]]

```

**Example 3:**

```
Input: root = []
Output: []

```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 2000]`.
- `1000 <= Node.val <= 1000`

```cpp
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int>> res;
        if(!root) return res;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            int qSz=q.size();
            vector<int> tmp;
            for(int i=0; i<qSz; i++){
                TreeNode* cur=q.front();q.pop();
                tmp.push_back(cur->val);
                if(cur->left){q.push(cur->left);}
                if(cur->right){q.push(cur->right);}
            }
            res.push_back(tmp);
        }
        reverse(res.begin(),res.end());
        return res;
    }
};
```