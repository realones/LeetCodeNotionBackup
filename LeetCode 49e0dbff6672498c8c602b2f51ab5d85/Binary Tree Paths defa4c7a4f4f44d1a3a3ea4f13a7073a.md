# Binary Tree Paths

#: 257
Difficult: Easy
Tags: Divide and Conquer, Tree, toNull
link: https://leetcode.com/problems/binary-tree-paths/
Priority: Low
Created time: July 8, 2022 4:26 PM
Last edited time: October 13, 2023 12:53 PM
practicedTimes: 1
source: jiuzhang, 算初Tree Divide&Conquer

Given the `root` of a binary tree, return *all root-to-leaf paths in **any order***.

A **leaf** is a node with no children.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/03/12/paths-tree.jpg](https://assets.leetcode.com/uploads/2021/03/12/paths-tree.jpg)

```
Input: root = [1,2,3,null,5]
Output: ["1->2->5","1->3"]

```

**Example 2:**

```
Input: root = [1]
Output: ["1"]

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 100]`.
- `100 <= Node.val <= 100`

```cpp
class Solution {
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> res;
        string tmp="";
        dfs(res,root,tmp);
        return res;
    }
    
    void dfs(vector<string>& res,TreeNode* root,string tmp){
        if(!root) {return;}
        if(tmp!="") tmp+="->";
        tmp+=to_string(root->val);
        if(!root->left && !root->right) {res.push_back(tmp); return;}
        
        dfs(res,root->left,tmp);
        dfs(res,root->right,tmp);
    }
    
};
```