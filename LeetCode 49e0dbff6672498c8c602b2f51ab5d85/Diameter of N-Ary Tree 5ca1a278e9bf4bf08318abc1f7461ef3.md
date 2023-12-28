# Diameter of N-Ary Tree

#: 1522
Difficult: Medium
Tags: DFS, Tree
link: https://leetcode.com/problems/diameter-of-n-ary-tree/description/
Priority: Medium
Status: HardToImplement
Created time: December 8, 2023 3:44 PM
Last edited time: December 8, 2023 3:45 PM
source: facebookFreq

Given a `root` of an `N-ary tree`, you need to compute the length of the diameter of the tree.

The diameter of an N-ary tree is the length of the **longest** path between any two nodes in the tree. This path may or may not pass through the root.

(*Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value.)*

**Example 1:**

![https://assets.leetcode.com/uploads/2020/07/19/sample_2_1897.png](https://assets.leetcode.com/uploads/2020/07/19/sample_2_1897.png)

```
Input: root = [1,null,3,2,4,null,5,6]
Output: 3
Explanation:Diameter is shown in red color.
```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/07/19/sample_1_1897.png](https://assets.leetcode.com/uploads/2020/07/19/sample_1_1897.png)

```
Input: root = [1,null,2,null,3,4,null,5,null,6]
Output: 4

```

**Example 3:**

![https://assets.leetcode.com/uploads/2020/07/19/sample_3_1897.png](https://assets.leetcode.com/uploads/2020/07/19/sample_3_1897.png)

```
Input: root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
Output: 7

```

**Constraints:**

- The depth of the n-ary tree is less than or equal to `1000`.
- The total number of nodes is between `[1, 104]`.

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
    int maxDep=0;
    TreeNode* lcaDeepestLeaves(TreeNode* root) {
        maxDep=maxDepth(root);
        return helper(root,1);
    }

    TreeNode* helper(TreeNode* node, int dep){
        if(!node) return NULL;
        if(dep==maxDep) return node;
        TreeNode* l=helper(node->left, dep+1);
        TreeNode* r=helper(node->right, dep+1);
        if(!l) return r;
        if(!r) return l;
        return node;
    }

    int maxDepth(TreeNode* root){
        if(!root) return 0;
        int l=maxDepth(root->left);
        int r=maxDepth(root->right);
        return max(l,r)+1;
    }
};

/*
find depth
if null return null
if depth == maxDep, return node
if l is #, return r
if r is #, return l
return node
*/
```