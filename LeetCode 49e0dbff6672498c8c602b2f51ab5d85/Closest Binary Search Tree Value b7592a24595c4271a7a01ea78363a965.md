# Closest Binary Search Tree Value

#: 270
Difficult: Easy
Tags: BST, BS_todo, DFS, Tree
link: https://leetcode.com/problems/closest-binary-search-tree-value/description/
Priority: Low
Created time: December 3, 2023 6:58 PM
Last edited time: December 3, 2023 6:59 PM
source: facebookFreq

Given the `root` of a binary search tree and a `target` value, return *the value in the BST that is closest to the* `target`. If there are multiple answers, print the smallest.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/03/12/closest1-1-tree.jpg](https://assets.leetcode.com/uploads/2021/03/12/closest1-1-tree.jpg)

```
Input: root = [4,2,5,1,3], target = 3.714286
Output: 4

```

**Example 2:**

```
Input: root = [1], target = 4.428571
Output: 1

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `0 <= Node.val <= 109`
- `109 <= target <= 109`

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
    int closestValue(TreeNode* root, double target) {
        TreeNode* cur=root;
        int res=-1;
        double minDiff=INT_MAX;
        while(cur){
            if(abs((double)cur->val - target) < minDiff){
                minDiff = abs((double)cur->val - target);
                res=cur->val;
            }
            else if(abs((double)cur->val - target) == minDiff){
                res=min(res,cur->val);
            }
            if((double)cur->val==target) return cur->val;
            else if(cur->val > target) cur=cur->left;
            else cur=cur->right;
        }
        return res;
    }
};
```