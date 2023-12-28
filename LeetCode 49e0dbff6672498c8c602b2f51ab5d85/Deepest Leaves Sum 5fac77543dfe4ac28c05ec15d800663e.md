# Deepest Leaves Sum

#: 1302
Difficult: Medium
Tags: BFS, Tree
link: https://leetcode.com/problems/deepest-leaves-sum/
Priority: Pass
Created time: August 12, 2023 4:47 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given the root of a binary tree, return *the sum of values of its deepest leaves*.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/07/31/1483_ex1.png](https://assets.leetcode.com/uploads/2019/07/31/1483_ex1.png)

```
Input: root = [1,2,3,4,5,null,6,7,null,null,null,null,8]
Output: 15

```

**Example 2:**

```
Input: root = [6,7,8,2,7,1,3,9,null,1,4,null,null,null,5]
Output: 19

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `1 <= Node.val <= 100`

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
    int deepestLeavesSum(TreeNode* root) {
        int res=0;
        if(!root) return 0;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            int sz=q.size();
            res=0;
            for(int i=0; i<sz; i++){
                TreeNode* cur=q.front(); q.pop();
                res+=cur->val;
                if(cur->left){q.push(cur->left);}
                if(cur->right){q.push(cur->right);}
            }
        }
        return res;
    }
}
```