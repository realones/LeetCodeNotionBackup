# Binary Tree Zigzag Level Order Traversal

#: 103
Difficult: Medium
Tags: BFS, Tree
link: https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/
Priority: Pass
Created time: July 17, 2022 11:52 PM
Last edited time: October 13, 2023 1:01 PM
practicedTimes: 1
source: LC_Top_Interview_150, jiuzhang, 算初BFS

Given the `root` of a binary tree, return *the zigzag level order traversal of its nodes' values*. (i.e., from left to right, then right to left for the next level and alternate between).

**Example 1:**

![https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[20,9],[15,7]]

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
- `100 <= Node.val <= 100`

```cpp
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> res;
        if(!root){return res;}
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
        bool reverseOrder=false;
        for(auto& tmp: res){
            if(reverseOrder){
                reverse(tmp.begin(),tmp.end());
            }
            reverseOrder=!reverseOrder;
        }
        return res;
    }
};
```