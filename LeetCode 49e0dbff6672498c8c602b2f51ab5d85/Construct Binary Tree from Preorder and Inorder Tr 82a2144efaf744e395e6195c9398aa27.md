# Construct Binary Tree from Preorder and Inorder Traversal

#: 105
Difficult: Medium
Tags: Tree
link: https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/
Priority: Low
Created time: July 15, 2022 12:17 AM
Last edited time: October 13, 2023 12:44 PM
practicedTimes: 1
source: LC_Top_Interview_150, jiuzhang, 算初Tree Divide&Conquer

Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return *the binary tree*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/02/19/tree.jpg](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]

```

**Example 2:**

```
Input: preorder = [-1], inorder = [-1]
Output: [-1]

```

**Constraints:**

- `1 <= preorder.length <= 3000`
- `inorder.length == preorder.length`
- `3000 <= preorder[i], inorder[i] <= 3000`
- `preorder` and `inorder` consist of **unique** values.
- Each value of `inorder` also appears in `preorder`.
- `preorder` is **guaranteed** to be the preorder traversal of the tree.
- `inorder` is **guaranteed** to be the inorder traversal of the tree.

```cpp
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        return helper(preorder,0,preorder.size()-1,inorder,0,inorder.size()-1);
    }
    TreeNode* helper(vector<int>& preorder,int pstart, int pend, vector<int>& inorder, int istart, int iend){
        //exist
        if(pstart>pend) return NULL;
        //process    
        int rootVal=preorder[pstart];
        TreeNode* root=new TreeNode(rootVal);
        int irootIndex=istart;
        for(;inorder[irootIndex]!=rootVal;irootIndex++);
        root->left=helper(preorder,pstart+1,pstart+(irootIndex-istart),inorder,istart,irootIndex-1);
        root->right=helper(preorder,pstart+(irootIndex-istart)+1,pend,inorder,irootIndex+1,iend);
            
        return root;
    }
};
```