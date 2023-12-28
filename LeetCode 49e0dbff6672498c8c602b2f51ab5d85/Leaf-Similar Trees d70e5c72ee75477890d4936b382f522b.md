# Leaf-Similar Trees

#: 872
Difficult: Easy
Tags: DFS, Iterate, Tree
link: https://leetcode.com/problems/leaf-similar-trees
Priority: High
Status: toSummarize
Created time: July 2, 2023 2:18 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_75

Consider all the leaves of a binary tree, from left to right order, the values of those leaves form a **leaf value sequence***.*

![https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/16/tree.png](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/16/tree.png)

For example, in the given tree above, the leaf value sequence is `(6, 7, 4, 9, 8)`.

Two binary trees are considered *leaf-similar* if their leaf value sequence is the same.

Return `true` if and only if the two given trees with head nodes `root1` and `root2` are leaf-similar.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/09/03/leaf-similar-1.jpg](https://assets.leetcode.com/uploads/2020/09/03/leaf-similar-1.jpg)

```
Input: root1 = [3,5,1,6,2,9,8,null,null,7,4], root2 = [3,5,1,6,7,4,2,null,null,null,null,null,null,9,8]
Output: true

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/09/03/leaf-similar-2.jpg](https://assets.leetcode.com/uploads/2020/09/03/leaf-similar-2.jpg)

```
Input: root1 = [1,2,3], root2 = [1,3,2]
Output: false

```

**Constraints:**

- The number of nodes in each tree will be in the range `[1, 200]`.
- Both of the given trees will have values in the range `[0, 200]`.

Solution:

最后一层，一开始想的bfs，发现不对，不管是dfs，tin, tpre, tpost, 应该都是从左到右的，都可以的。

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
    bool leafSimilar(TreeNode* root1, TreeNode* root2) {
        vector<int> seq1=helper(root1);
        vector<int> seq2=helper(root2);
        if(seq1.size()!=seq2.size()) return false;
        for(int i=0; i<seq1.size(); i++){
            if(seq1[i]!=seq2[i]) return false;
        }
        return true;
    }

    vector<int> helper(TreeNode* root){
        vector<int> res;
        if(!root) return {};
        stack<TreeNode*> stk;
        stk.push(root);
        while(!stk.empty()){
            TreeNode* cur=stk.top();stk.pop();
            if(!cur->left && !cur->right) res.push_back(cur->val);
            if(cur->right) stk.push(cur->right);
            if(cur->left) stk.push(cur->left);
        }
        return res;
    }
};
```