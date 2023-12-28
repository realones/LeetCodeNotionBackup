# Invert Binary Tree

#: 226
Difficult: Easy
Tags: Tree
link: https://leetcode.com/problems/invert-binary-tree/
Priority: Pass
Created time: June 6, 2023 7:53 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

Given the `root` of a binary tree, invert the tree, and return *its root*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

```
Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/03/14/invert2-tree.jpg](https://assets.leetcode.com/uploads/2021/03/14/invert2-tree.jpg)

```
Input: root = [2,1,3]
Output: [2,3,1]

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
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if(!root) return NULL;
        helper(root);
        return root;
    }
    TreeNode* helper(TreeNode* root){
        if(!root) return NULL;
        TreeNode* l=helper(root->left);
        TreeNode* r=helper(root->right);
        root->left=r;
        root->right=l;
        return root;
    }
```