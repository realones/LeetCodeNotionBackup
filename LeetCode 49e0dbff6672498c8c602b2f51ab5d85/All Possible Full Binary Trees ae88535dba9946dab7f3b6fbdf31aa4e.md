# All Possible Full Binary Trees

#: 894
Difficult: Medium
Tags: DP, Tree, recursion
link: https://leetcode.com/problems/all-possible-full-binary-trees
Priority: Medium
Status: More Solution
Created time: July 22, 2023 6:16 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

Given an integer `n`, return *a list of all possible **full binary trees** with* `n` *nodes*. Each node of each tree in the answer must have `Node.val == 0`.

Each element of the answer is the root node of one possible tree. You may return the final list of trees in **any order**.

A **full binary tree** is a binary tree where each node has exactly `0` or `2` children.

**Example 1:**

![https://s3-lc-upload.s3.amazonaws.com/uploads/2018/08/22/fivetrees.png](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/08/22/fivetrees.png)

```
Input: n = 7
Output: [[0,0,0,null,null,0,0,null,null,0,0],[0,0,0,null,null,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,null,null,null,null,0,0],[0,0,0,0,0,null,null,0,0]]

```

**Example 2:**

```
Input: n = 3
Output: [[0,0,0]]

```

**Constraints:**

- `1 <= n <= 20`

todo: see editorial for more solution

Solution:

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
    vector<TreeNode*> allPossibleFBT(int n) {//n:1~20
        if(n%2==0) return {};
        vector<vector<TreeNode*>> dp(n+1); //k ~ all sub solutions
        dp[1]={new TreeNode(0)};
        
        return helper(dp,n);
    }

    vector<TreeNode*> helper(vector<vector<TreeNode*>>& dp, int k){
        if(!dp[k].empty()) return dp[k];
        //if(n==1) covered in above
        for(int lcnt=1,rcnt; rcnt=k-1-lcnt,lcnt<=k-2;lcnt+=2){
            vector<TreeNode*> l=helper(dp,lcnt);
            vector<TreeNode*> r=helper(dp,rcnt);
            for(int i=0; i<l.size(); i++){
                for(int j=0; j<r.size(); j++){
                    TreeNode* root=new TreeNode(0);
                    root->left=l[i];
                    root->right=r[j];
                    dp[k].push_back(root);
                }
            }
        }
        return dp[k];
    }

    /*
    TreeNode* cloneTree(TreeNode* node){
        if(!node) return NULL;
        TreeNode* clone=new TreeNode(node->val);
        clone->left=cloneTree(node->left);
        clone->right=cloneTree(node->right);
        return clone;
    }
    */
};

/*
all posible ones - recur
tree with n nodes
each node has 0/2 children

(x+y+1)=n
1 as root
left x items->dfs, x:1,3,5,7,...
right y items->dfs y:1,3,5,...

*/
```