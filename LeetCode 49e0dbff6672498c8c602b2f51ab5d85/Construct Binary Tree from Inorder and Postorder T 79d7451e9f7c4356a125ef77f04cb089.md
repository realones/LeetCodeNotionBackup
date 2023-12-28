# Construct Binary Tree from Inorder and Postorder Traversal

#: 106
Difficult: Medium
Tags: Divide and Conquer, Tree
link: https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/
Priority: Low
Created time: June 5, 2023 2:04 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

Given two integer arrays `inorder` and `postorder` where `inorder` is the inorder traversal of a binary tree and `postorder` is the postorder traversal of the same tree, construct and return *the binary tree*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/02/19/tree.jpg](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
Input: inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
Output: [3,9,20,null,null,15,7]

```

**Example 2:**

```
Input: inorder = [-1], postorder = [-1]
Output: [-1]

```

**Constraints:**

- `1 <= inorder.length <= 3000`
- `postorder.length == inorder.length`
- `3000 <= inorder[i], postorder[i] <= 3000`
- `inorder` and `postorder` consist of **unique** values.
- Each value of `postorder` also appears in `inorder`.
- `inorder` is **guaranteed** to be the inorder traversal of the tree.
- `postorder` is **guaranteed** to be the postorder traversal of the tree.

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
    vector<int> inorder;
    vector<int> postorder;
    
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        this->inorder=inorder;
        this->postorder=postorder;
        int n=inorder.size();
        
        return helper(0,n-1,0,n-1);
    }
    
    TreeNode* helper(int is, int ie, int ps, int pe){
        if(is>ie || ps>pe) return NULL;
        int rootVal=postorder[pe];
        TreeNode* root=new TreeNode(rootVal);
        int iRootIdx;
        for(int i=is;i<=ie;i++){
            if(inorder[i]==rootVal){
                iRootIdx=i; break;
            }
        }
        int lsz=iRootIdx-is;
        root->left=helper(is,iRootIdx-1,ps,ps+lsz-1);
        root->right=helper(iRootIdx+1,ie,ps+lsz,pe-1);
        return root;
    }
};

//in:    l root r
//post:  l r root
```