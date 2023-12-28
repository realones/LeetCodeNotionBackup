# Binary Tree Level Order Traversal

#: 102
Difficult: Medium
Tags: BFS, Tree, tbfs_preChkNull_wSz
link: https://leetcode.com/problems/binary-tree-level-order-traversal/
Priority: Low
Created time: July 15, 2022 12:17 AM
Last edited time: November 17, 2023 1:27 AM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, jiuzhang, ticktokFreq, 算初BFS, 算初Tree Divide&Conquer

Given the `root` of a binary tree, return *the level order traversal of its nodes' values*. (i.e., from left to right, level by level).

**Example 1:**

![https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]

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
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        if(!root) return res;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            int sz=q.size();
            vectoc<int> tmp;
            for(int i=0; i<sz; i++){
                TreeNode* cur = q.front(); q.pop();
                tmp.push_back(cur->val);
                if(cur->left){q.push(cur->left);}
                if(cur->right){q.push(cur->right);}
            }
            res.push_back(tmp);
        }
        return res;
    }
};
```