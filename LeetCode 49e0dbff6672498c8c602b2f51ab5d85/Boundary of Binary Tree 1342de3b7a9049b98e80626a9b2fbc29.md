# Boundary of Binary Tree

#: 545
Difficult: Medium
Tags: DFS, Tree
link: https://leetcode.com/problems/boundary-of-binary-tree/description/
Priority: Medium
Created time: December 8, 2023 11:29 PM
Last edited time: December 8, 2023 11:31 PM
source: facebookFreq

The **boundary** of a binary tree is the concatenation of the **root**, the **left boundary**, the **leaves** ordered from left-to-right, and the **reverse order** of the **right boundary**.

The **left boundary** is the set of nodes defined by the following:

- The root node's left child is in the left boundary. If the root does not have a left child, then the left boundary is **empty**.
- If a node in the left boundary and has a left child, then the left child is in the left boundary.
- If a node is in the left boundary, has **no** left child, but has a right child, then the right child is in the left boundary.
- The leftmost leaf is **not** in the left boundary.

The **right boundary** is similar to the **left boundary**, except it is the right side of the root's right subtree. Again, the leaf is **not** part of the **right boundary**, and the **right boundary** is empty if the root does not have a right child.

The **leaves** are nodes that do not have any children. For this problem, the root is **not** a leaf.

Given the `root` of a binary tree, return *the values of its **boundary***.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/11/11/boundary1.jpg](https://assets.leetcode.com/uploads/2020/11/11/boundary1.jpg)

```
Input: root = [1,null,2,3,4]
Output: [1,3,4,2]
Explanation:
- The left boundary is empty because the root does not have a left child.
- The right boundary follows the path starting from the root's right child 2 -> 4.
  4 is a leaf, so the right boundary is [2].
- The leaves from left to right are [3,4].
Concatenating everything results in [1] + [] + [3,4] + [2] = [1,3,4,2].

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/11/11/boundary2.jpg](https://assets.leetcode.com/uploads/2020/11/11/boundary2.jpg)

```
Input: root = [1,2,3,4,5,6,null,null,null,7,8,9,10]
Output: [1,2,4,7,8,9,10,6,3]
Explanation:
- The left boundary follows the path starting from the root's left child 2 -> 4.
  4 is a leaf, so the left boundary is [2].
- The right boundary follows the path starting from the root's right child 3 -> 6 -> 10.
  10 is a leaf, so the right boundary is [3,6], and in reverse order is [6,3].
- The leaves from left to right are [4,7,8,9,10].
Concatenating everything results in [1] + [2] + [4,7,8,9,10] + [6,3] = [1,2,4,7,8,9,10,6,3].

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `1000 <= Node.val <= 1000`

comment:

desc is so hard to understand

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
    vector<int> leaves;
    vector<int> boundaryOfBinaryTree(TreeNode* root) {
        if(!root) return {};
        vector<int> res;
        //left
        vector<int> left=getLeft(root->left);
        //right
        vector<int> right=getRight(root->right);
        //leaves
        if(root->left || root->right)
            getLeaves(root);
        //add all to res
        res.push_back(root->val);
        res.insert(res.end(),left.begin(),left.end());
        res.insert(res.end(),leaves.begin(),leaves.end());
        res.insert(res.end(),right.begin(),right.end());
        return res;
    }
    vector<int> getLeft(TreeNode* node){
        vector<int> res;
        if(!node) return {};
        while(node->left || node->right){//node not leaf
            res.push_back(node->val);
            if(node->left){
                node=node->left;
            }
            else if(node->right){
                node=node->right;
            }
        }
        return res;
    }
    vector<int> getRight(TreeNode* node){
        vector<int> res;
        if(!node) return {};
        while(node->left || node->right){//node not leaf
            res.push_back(node->val);
            if(node->right){
                node=node->right;
            }
            else if(node->left){
                node=node->left;
            }
        }
        reverse(res.begin(), res.end());
        return res;
    }
    void getLeaves(TreeNode* node){
        if(!node) return;
        if(!node->left && !node->right) {leaves.push_back(node->val); return;}
        getLeaves(node->left);
        getLeaves(node->right);
    }
};
```