# Lowest Common Ancestor of a Binary Tree II

#: 1644
Difficult: Medium
Tags: DFS, Tree
link: https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree-ii/
Priority: Medium
Status: More Solution
Created time: July 14, 2023 10:36 PM
Last edited time: October 14, 2023 10:44 PM
source: leetcode_daily
related: Lowest Common Ancestor of a Binary Tree (Lowest%20Common%20Ancestor%20of%20a%20Binary%20Tree%20358623c63e324e6e9d5de06b9aa3ec65.md)

Given the `root` of a binary tree, return *the lowest common ancestor (LCA) of two given nodes,* `p` *and* `q`. If either node `p` or `q` **does not exist** in the tree, return `null`. All values of the nodes in the tree are **unique**.

According to the **[definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor)**: "The lowest common ancestor of two nodes `p` and `q` in a binary tree `T` is the lowest node that has both `p` and `q` as **descendants** (where we allow **a node to be a descendant of itself**)". A **descendant** of a node `x` is a node `y` that is on the path from node `x` to some leaf node.

**Example 1:**

![https://assets.leetcode.com/uploads/2018/12/14/binarytree.png](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```

**Example 2:**

![https://assets.leetcode.com/uploads/2018/12/14/binarytree.png](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5. A node can be a descendant of itself according to the definition of LCA.
```

**Example 3:**

![https://assets.leetcode.com/uploads/2018/12/14/binarytree.png](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 10
Output: null
Explanation: Node 10 does not exist in the tree, so return null.

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `109 <= Node.val <= 109`
- All `Node.val` are **unique**.
- `p != q`

**Follow up:**

Can you find the LCA traversing the tree, without checking nodes existence?

todo: follow up

Solution:

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int visited=0;
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        TreeNode* res=lowestCommonAncestor2(root, p, q);
        std::cout << "visited" << " -- " << visited << std::endl;
        return visited==2?res:NULL;
    }
    TreeNode* lowestCommonAncestor2(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(!root) return NULL;
        TreeNode* l=lowestCommonAncestor2(root->left, p, q);
        TreeNode* r=lowestCommonAncestor2(root->right, p, q);
        if(root==p || root==q) {visited++; return root;}
        if(l&&r) return root;
        if(l) return l;
        if(r) return r;
        return NULL;
    }
}
```