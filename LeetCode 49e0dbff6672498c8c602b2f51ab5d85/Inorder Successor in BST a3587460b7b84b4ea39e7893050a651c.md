# Inorder Successor in BST

#: 285
Difficult: Medium
Tags: 1Path, Tree
link: https://leetcode.com/problems/inorder-successor-in-bst/
Priority: High
Created time: July 12, 2022 10:02 PM
Last edited time: October 14, 2023 10:28 PM
practicedTimes: 1
source: jiuzhang, 算初Tree Divide&Conquer

leet locked, 

[https://www.lintcode.com/problem/inorder-successor-in-bst](https://www.lintcode.com/problem/inorder-successor-in-bst)

Given the `root` of a binary search tree and a node `p` in it, return *the in-order successor of that node in the BST*. If the given node has no in-order successor in the tree, return `null`.

The successor of a node `p` is the node with the smallest key greater than `p.val`.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/01/23/285_example_1.PNG](https://assets.leetcode.com/uploads/2019/01/23/285_example_1.PNG)

```
Input: root = [2,1,3], p = 1
Output: 2
Explanation: 1's in-order successor node is 2. Note that both p and the return value is of TreeNode type.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2019/01/23/285_example_2.PNG](https://assets.leetcode.com/uploads/2019/01/23/285_example_2.PNG)

```
Input: root = [5,3,6,2,4,null,null,1], p = 6
Output: null
Explanation: There is no in-order successor of the current node, so the answer isnull.

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `105 <= Node.val <= 105`

O(h)

BST find 1st node val larger than p

```cpp
class Solution {
public:
    TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
        if(!root || !p) return NULL;
        TreeNode* res=NULL, *cur=root;
        while(cur){
            if(cur->val <= p->val){
                cur=cur->right;
            }
            else{
                res=cur;
                cur=cur->left;
            }
        }
        return res;
    }
};
```