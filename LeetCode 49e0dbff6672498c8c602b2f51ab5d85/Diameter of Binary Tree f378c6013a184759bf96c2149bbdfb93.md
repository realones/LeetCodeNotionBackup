# Diameter of Binary Tree

#: 687
Difficult: Medium
Tags: DFS, Tree
link: https://leetcode.com/problems/longest-univalue-path/description/
Priority: Medium
Created time: June 27, 2023 11:51 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Diameter of Binary Tree (Diameter%20of%20Binary%20Tree%20a3c0379b41174057a7e2a11333f8d905.md), Binary Tree Maximum Path Sum (Binary%20Tree%20Maximum%20Path%20Sum%20f544ed88691540c5af96eba3bb262c14.md)

Given the `root` of a binary tree, return *the length of the longest path, where each node in the path has the same value*. This path may or may not pass through the root.

**The length of the path** between two nodes is represented by the number of edges between them.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/10/13/ex1.jpg](https://assets.leetcode.com/uploads/2020/10/13/ex1.jpg)

```
Input: root = [5,4,5,1,1,null,5]
Output: 2
Explanation: The shown image shows that the longest path of the same value (i.e. 5).

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/10/13/ex2.jpg](https://assets.leetcode.com/uploads/2020/10/13/ex2.jpg)

```
Input: root = [1,4,5,4,4,null,5]
Output: 2
Explanation: The shown image shows that the longest path of the same value (i.e. 4).

```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `1000 <= Node.val <= 1000`
- The depth of the tree will not exceed `1000`.

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
    int res=0;
    int longestUnivaluePath(TreeNode* root) {
        if(!root) return 0;
        helper(root);
        return res;
    }

    //longest one side from root with node_val = root_val, also handle update res
    int helper(TreeNode* root){
        if(!root) return -1;
        int l=helper(root->left);
        if(!root->left || root->left->val != root->val) l=-1;
        int r=helper(root->right);
        if(!root->right || root->right->val != root->val) r=-1;
        res=max(res,l+r+2);
        return max(l,r)+1;
    }
}
```