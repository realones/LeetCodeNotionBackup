# Count Univalue Subtrees

#: 250
Difficult: Medium
Tags: DFS, Divide and Conquer, Tree
link: https://leetcode.com/problems/count-univalue-subtrees/description/
Priority: Medium
Status: HardToImplement
Created time: June 28, 2023 5:26 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

Given the `root` of a binary tree, return *the number of **uni-value***

*subtrees*

.

A **uni-value subtree** means all nodes of the subtree have the same value.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/08/21/unival_e1.jpg](https://assets.leetcode.com/uploads/2020/08/21/unival_e1.jpg)

```
Input: root = [5,1,5,5,5,null,5]
Output: 4

```

**Example 2:**

```
Input: root = []
Output: 0

```

**Example 3:**

```
Input: root = [5,5,5,5,5,null,5]
Output: 6

```

**Constraints:**

- The number of the node in the tree will be in the range `[0, 1000]`.
- `1000 <= Node.val <= 1000`

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
using ii=pair<int,int>;

class Solution {
public:
    int res=0;
    int countUnivalSubtrees(TreeNode* root) {
        helper(root);
        return res;
    }

    //return {cnt,uni_val}
    //if NULL, return {0,0}, means cnt=0
    //cnt==-1 means invalid
    ii helper(TreeNode* root){
        if(!root) return {0,0};
        auto [lcnt,lval]=helper(root->left);
        auto [rcnt,rval]=helper(root->right);
        if(lcnt==-1 || rcnt==-1) return {-1,0};
        if(lcnt==0) lval=root->val;
        if(rcnt==0) rval=root->val;
        if(lval==rval && rval==root->val) {
            res+=1;
            return{lcnt+rcnt+1,lval};
        }
        return {-1,0};
    }
};
```