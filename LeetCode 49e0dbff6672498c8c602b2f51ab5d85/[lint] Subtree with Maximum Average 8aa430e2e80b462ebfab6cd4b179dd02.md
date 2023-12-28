# [lint] Subtree with Maximum Average

#: 597
Difficult: Easy
Tags: Divide and Conquer, Tree, toNull
link: https://www.lintcode.com/problem/subtree-with-maximum-average
Priority: Low
Created time: July 8, 2022 4:26 PM
Last edited time: October 13, 2023 12:55 PM
practicedTimes: 1
source: jiuzhang, 算初Tree Divide&Conquer

Given a binary tree, find the subtree with maximum average. Return the root of the subtree.

Example:
**Example 1**

```
Input：
{1,-5,11,1,2,4,-2}
Output：11
Explanation:
The tree is look like this:
    1
  /   \\
-5     11
/ \\   /  \\
1   2 4    -2
The average of subtree of 11 is 4.3333, is the maximun.

```

**Example 2**

```
Input：
{1,-5,11}
Output：11
Explanation:
    1
  /   \\
-5     11
The average of subtree of 1,-5,11 is 2.333,-5,11. So the subtree of 11 is the maximun.

```

Challenge

Related Questions:
binary-tree-maximum-node,maximum-subtree,minimum-subtree

Tags:
Amazon,Binary Tree,Depth-first Search

```cpp
class Solution {
public:
    /*
     * @param root: the root of binary tree
     * @return: the root of the maximum average of subtree
     */
    TreeNode * res=NULL;
    double maxAve=INT_MIN;
    typedef pair<int,int> Block;//cnt, sum

    TreeNode * findSubtree2(TreeNode * root) {
        if(!root) return NULL;
        helper(root);
        return res;
    }

    Block helper(TreeNode *root){
        if(!root) return Block(0,0);

        Block l=helper(root->left);
        Block r=helper(root->right);
        int cnt=l.first+r.first+1;
        int sum=l.second+r.second+root->val;
        double ave=(double)sum/cnt;
        if(ave>=maxAve){
            maxAve=ave;
            res=root;
        }

        return Block(cnt,sum);
    }
};
```