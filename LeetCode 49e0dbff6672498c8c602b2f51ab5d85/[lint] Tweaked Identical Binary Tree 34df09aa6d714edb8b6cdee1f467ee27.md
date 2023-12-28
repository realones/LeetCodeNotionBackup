# [lint] Tweaked Identical Binary Tree

#: 470
Difficult: Easy
Tags: Tree
link: https://www.lintcode.com/problem/tweaked-identical-binary-tree
Priority: Pass
Created time: July 15, 2022 12:17 AM
Last edited time: October 12, 2023 3:03 PM
practicedTimes: 1
source: jiuzhang, 算初Tree Divide&Conquer

Check two given binary trees are identical or not. Assuming any number of *tweaks* are allowed. A tweak is defined as a swap of the children of one node in the tree.

Example:
**Example 1:**

```
Input:{1,2,3,4},{1,3,2,#,#,#,4}
Output:true
Explanation:
       1             1
      / \\           / \\
     2   3   and   3   2
    /                   \\
   4                     4

are identical.

```

**Example 2:**

```
Input:{1,2,3,4},{1,3,2,4}
Output:false
Explanation:

       1             1
      / \\           / \\
     2   3   and   3   2
    /             /
   4             4

are not identical.

```

Challenge
O(n) time

Related Questions:
same-tree

```cpp
class Solution {
public:
    /**
     * @param a: the root of binary tree a.
     * @param b: the root of binary tree b.
     * @return: true if they are tweaked identical, or false.
     */
    bool isTweakedIdentical(TreeNode * a, TreeNode * b) {
        // write your code here
        if(!a && !b) return true;
        if(!a || !b) return false;
        if(a->val != b->val) return false;
        return isTweakedIdentical(a->left,b->right) && isTweakedIdentical(a->right,b->left) || isTweakedIdentical(a->left,b->left) && isTweakedIdentical(a->right,b->right);
    }
};
```