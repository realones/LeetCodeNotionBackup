# [lint] Minimum Subtree

#: 596
Difficult: Easy
Tags: Divide and Conquer, Tree, toNull
link: https://www.lintcode.com/problem/minimum-subtree/
Priority: Low
Created time: July 8, 2022 4:26 PM
Last edited time: October 13, 2023 12:54 PM
practicedTimes: 1
source: jiuzhang, 算初Tree Divide&Conquer

Given a binary tree, find the subtree with minimum sum. Return the root of the subtree.

Example:
Example 1:

```
Input:
{1,-5,2,1,2,-4,-5}
Output:1
Explanation:
The tree is look like this:
    1
  /   \
-5     2
/ \   /  \
1  2 -4  -5
The sum of whole tree is minimum, so return the root.

```

Example 2:

```
Input:
{1}
Output:1
Explanation:
The tree is look like this:
  1
There is one and only one subtree in the tree. So we return 1.

```

Challenge

Related Questions:
binary-tree-maximum-node,maximum-subtree,subtree-with-maximum-average,binary-tree-longest-consecutive-sequence

Tags:
Microsoft,Depth-first Search,Yelp,Binary Tree

```cpp
class Solution {
public:
    /*
     * @param root: the root of binary tree
     * @return: the root of the minimum subtree
     */
    TreeNode* res=NULL;
    int minSum=INT_MAX;

    TreeNode * findSubtree(TreeNode * root) {
        if(root==NULL){return NULL;}
        helper(root);
        return res;
    }

    int helper(TreeNode* root){
        if(root==NULL){return 0;}

        int l=helper(root->left);
        int r=helper(root->right);
        //find the min
        int c=l+r+root->val;
        if(c<minSum){
            res=root;
            minSum=c;
        }
        return c;
    }
};
```