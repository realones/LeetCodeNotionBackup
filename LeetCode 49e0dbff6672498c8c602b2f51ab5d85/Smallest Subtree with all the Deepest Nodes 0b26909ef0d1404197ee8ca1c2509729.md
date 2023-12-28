# Smallest Subtree with all the Deepest Nodes

#: 865
Difficult: Medium
Tags: BFS, DFS, Hash, Tree
link: https://leetcode.com/problems/smallest-subtree-with-all-the-deepest-nodes/
Priority: Low
Status: More Solution
Created time: June 27, 2023 11:48 PM
Last edited time: December 8, 2023 5:47 PM
source: HuaHua, facebookFreq

Given the `root` of a binary tree, the depth of each node is **the shortest distance to the root**.

Return *the smallest subtree* such that it contains **all the deepest nodes** in the original tree.

A node is called **the deepest** if it has the largest depth possible among any node in the entire tree.

The **subtree** of a node is a tree consisting of that node, plus the set of all descendants of that node.

**Example 1:**

![https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/01/sketch1.png](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/01/sketch1.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4]
Output: [2,7,4]
Explanation: We return the node with value 2, colored in yellow in the diagram.
The nodes coloured in blue are the deepest nodes of the tree.
Notice that nodes 5, 3 and 2 contain the deepest nodes in the tree but node 2 is the smallest subtree among them, so we return it.

```

**Example 2:**

```
Input: root = [1]
Output: [1]
Explanation: The root is the deepest node in the tree.

```

**Example 3:**

```
Input: root = [0,1,3,null,2]
Output: [2]
Explanation: The deepest node in the tree is 2, the valid subtrees are the subtrees of nodes 2, 1 and 0 but the subtree of node 2 is the smallest.

```

**Constraints:**

- The number of nodes in the tree will be in the range `[1, 500]`.
- `0 <= Node.val <= 500`
- The values of the nodes in the tree are **unique**.

**Note:** This question is the same as 1123: [https://leetcode.com/problems/lowest-common-ancestor-of-deepest-leaves/](https://leetcode.com/problems/lowest-common-ancestor-of-deepest-leaves/)

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
    int maxDepth=0;
    TreeNode* subtreeWithAllDeepest(TreeNode* root) {
        maxDepth=getMaxDepth(root);
        return helper(root,1);
    }

    TreeNode* helper(TreeNode* node, int d){
        if(!node) return NULL;
        if(d==maxDepth) return node;
        TreeNode* l=helper(node->left, d+1);
        TreeNode* r=helper(node->right, d+1);
        if(!r) return l;
        if(!l) return r;
        return node;
    }

    int getMaxDepth(TreeNode* node){
        if(!node) return 0;
        int l=getMaxDepth(node->left);
        int r=getMaxDepth(node->right);
        return max(l,r)+1;
    }
};

/*
find maxDep
recurrsive:
    if # return #
    if dep==maxDep, return node
    if !l, return r
    if !r, return l
    return node
*/
```