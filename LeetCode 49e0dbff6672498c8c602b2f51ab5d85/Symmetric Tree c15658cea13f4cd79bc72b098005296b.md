# Symmetric Tree

#: 101
Difficult: Easy
Tags: Tree
link: https://leetcode.com/problems/symmetric-tree/
Priority: Pass
Created time: June 6, 2023 7:51 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_Top_Interview_150

Given the `root` of a binary tree, *check whether it is a mirror of itself* (i.e., symmetric around its center).

**Example 1:**

![https://assets.leetcode.com/uploads/2021/02/19/symtree1.jpg](https://assets.leetcode.com/uploads/2021/02/19/symtree1.jpg)

```
Input: root = [1,2,2,3,4,4,3]
Output: true

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/02/19/symtree2.jpg](https://assets.leetcode.com/uploads/2021/02/19/symtree2.jpg)

```
Input: root = [1,2,2,null,3,null,3]
Output: false

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 1000]`.
- `100 <= Node.val <= 100`

**Follow up:**

Could you solve it both recursively and iteratively?

```cpp
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(root==NULL) return true;
        else{return compare(root->left,root->right);}
        
    }
    bool compare(TreeNode* node1,TreeNode* node2){
        if(node1==NULL && node2== NULL) return true;
        else if(node1==NULL || node2== NULL) return false;
        else if(node1->val!=node2->val) return false;
        else{
            return compare(node1->left,node2->right)&&compare(node1->right,node2->left);
        }
    }
};
```