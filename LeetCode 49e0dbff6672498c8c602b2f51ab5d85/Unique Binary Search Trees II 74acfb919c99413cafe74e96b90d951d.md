# Unique Binary Search Trees II

#: 95
Difficult: Medium
Tags: BST, BackTracking, DP, Tree
link: https://leetcode.com/problems/unique-binary-search-trees-ii/description/
Priority: Medium
Status: More Solution
Created time: August 5, 2023 11:44 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

Given an integer `n`, return *all the structurally unique **BST'**s (binary search trees), which has exactly* `n` *nodes of unique values from* `1` *to* `n`. Return the answer in **any order**.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg](https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg)

```
Input: n = 3
Output: [[1,null,2,null,3],[1,null,3,2],[2,1,3],[3,1,null,null,2],[3,2,null,1]]

```

**Example 2:**

```
Input: n = 1
Output: [[1]]

```

**Constraints:**

- `1 <= n <= 8`

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
    vector<TreeNode*> generateTrees(int n) {
        return helper(1,n);
    }
    vector<TreeNode*> helper(int start, int end) {
        vector<TreeNode*> res;
        if(start>end) return {NULL};
        for(int k=start;k<=end;k++){
            vector<TreeNode*> ltrees = helper(start,k-1);
            vector<TreeNode*> rtrees = helper(k+1,end);
            for(auto l: ltrees){
                for(auto r: rtrees){
                    TreeNode* root=new TreeNode(k);
                    root->left=l;
                    root->right=r;
                    res.push_back(root);
                }
            }
        }
        return res;
    }
};
```