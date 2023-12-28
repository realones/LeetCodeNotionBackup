# [lint] Search Range in Binary Search Tree

#: 11
Difficult: Medium
Tags: DFS, Tree
link: https://www.lintcode.com/problem/11/
Priority: High
Created time: July 12, 2022 10:02 PM
Last edited time: October 14, 2023 10:32 PM
practicedTimes: 1
source: jiuzhang, 算初Tree Divide&Conquer

**Description**

Given a binary search tree and a range `[k1, k2]`, return node values within a given range in ascending order.

**Example**

**Example 1:**

Input:

```
tree = {5}
k1 = 6
k2 = 10

```

Output:

```
[]

```

Explanation:

No number between 6 and 10

**Example 2:**

Input:

```
tree = {20,8,22,4,12}
k1 = 10
k2 = 22

```

Output:

```
[12,20,22]

```

Explanation:

[12,20,22] between 10 and 22

inorder iterate可以解决，但是iterate了没有必要的node

image BST is sorted ary, and image the range array slide on the BST ary
not just find one, so recursive maintaining the result ary

inorder DFS

```cpp
class Solution {
public:
    /**
     * @param root: param root: The root of the binary search tree
     * @param k1: An integer
     * @param k2: An integer
     * @return: return: Return all keys that k1<=key<=k2 in ascending order
     */
    vector<int> searchRange(TreeNode * root, int k1, int k2) {
        // write your code here
        vector<int> res;
        if(!root) return res;
        helper(root,res,k1,k2);
        return res;
    }
    void helper(TreeNode* root, vector<int>& res, int k1, int k2){
        if(!root) return;
        if(root->val>k1) helper(root->left,res,k1,k2);
        if(root->val>=k1 && root->val<=k2) res.push_back(root->val);
        if(root->val<k2) helper(root->right,res,k1,k2);
    }
};
```