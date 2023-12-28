# Binary Tree Right Side View

#: 199
Difficult: Medium
Tags: BFS, Tree
link: https://leetcode.com/problems/binary-tree-right-side-view/
Priority: Low
Created time: June 5, 2023 5:21 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_75, LC_Top_Interview_150

Given the `root` of a binary tree, imagine yourself standing on the **right side** of it, return *the values of the nodes you can see ordered from top to bottom*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/02/14/tree.jpg](https://assets.leetcode.com/uploads/2021/02/14/tree.jpg)

```
Input: root = [1,2,3,null,5,null,4]
Output: [1,3,4]

```

**Example 2:**

```
Input: root = [1,null,3]
Output: [1,3]

```

**Example 3:**

```
Input: root = []
Output: []

```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 100]`.
- `100 <= Node.val <= 100`

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
    vector<int> rightSideView(TreeNode* root) {
        vector<int> res;
        if(!root) return res;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            int sz=q.size();
            int last=0;
            for(int i=0; i<sz; i++){
                TreeNode* cur=q.front(); q.pop();
                last=cur->val;
                if(cur->left){q.push(cur->left);}
                if(cur->right){q.push(cur->right);}
            }
            res.push_back(last); //process
        }
        return res;
    }
};
```