# Find Largest Value in Each Tree Row

#: 515
Difficult: Medium
Tags: BFS, Tree
link: https://leetcode.com/problems/find-largest-value-in-each-tree-row/
Priority: Pass
Created time: October 23, 2023 10:02 PM
Last edited time: October 23, 2023 10:03 PM
source: leetcode_daily

Given the `root` of a binary tree, return *an array of the largest value in each row* of the tree **(0-indexed)**.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/08/21/largest_e1.jpg](https://assets.leetcode.com/uploads/2020/08/21/largest_e1.jpg)

```
Input: root = [1,3,2,5,3,null,9]
Output: [1,3,9]

```

**Example 2:**

```
Input: root = [1,2,3]
Output: [1,3]

```

**Constraints:**

- The number of nodes in the tree will be in the range `[0, 104]`.
- `231 <= Node.val <= 231 - 1`

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
    vector<int> largestValues(TreeNode* root) {
        vector<int> res;
        if(!root) return res;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            int sz=q.size();
            int tmp=INT_MIN;
            for(int i=0; i<sz; i++){
                TreeNode* cur=q.front(); q.pop();
                tmp=max(tmp,cur->val);
                if(cur->left){q.push(cur->left);}
                if(cur->right){q.push(cur->right);}
            }
            res.push_back(tmp); //process
        }
        return res;
    }
};
```