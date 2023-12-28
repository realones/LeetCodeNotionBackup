# [lint] Insert Node in a Binary Search Tree

#: 85
Difficult: Easy
Tags: 1Path, Tree
link: https://www.lintcode.com/problem/85/
Priority: Low
Created time: July 15, 2022 12:07 AM
Last edited time: October 13, 2023 12:56 PM
practicedTimes: 1
source: jiuzhang, 算初Tree Divide&Conquer

**Description**

Given a binary search tree and a new tree node, insert the node into the tree. You should keep the tree still be a valid binary search tree.

You can assume there is no duplicate values in this tree + node.

**Example**

**Example 1:**

Input:

```
tree = {}
node= 1

```

Output:

```
{1}

```

Explanation:

Insert node 1 into the empty tree, so there is only one node on the tree.

**Example 2:**

Input:

```
tree = {2,1,4,#,#,3}
node = 6
```

Output:

```
{2,1,4,#,#,3,6}
```

Explanation:

2                              2

/   \                          /   \

1     4          -->       1       4

/                                /  \

3                                3      6

**Challenge**

Can you do it without recursion?

can always insert as a leaf

```cpp
//solution 2: iteratively, actually is similar to recursively solution
//add node to bottom of the tree
class Solution {
public:
    /*
     * @param root: The root of the binary search tree.
     * @param node: insert this node into the binary search tree
     * @return: The root of the new binary search tree.
     */
    TreeNode * insertNode(TreeNode * root, TreeNode * node) {
        if(!root) return node;
        
        TreeNode* cur=root;
        while(1){
            if(node->val < cur->val){
                if(!cur->left) {cur->left=node; return root;}
                cur=cur->left;
            }
            else{
                if(!cur->right) {cur->right=node; return root;}
                cur=cur->right;
            }
        }
    }
};
```

```cpp
//solution 1 recursive
//always insert to bottom of the tree
class Solution {
public:
    /*
     * @param root: The root of the binary search tree.
     * @param node: insert this node into the binary search tree
     * @return: The root of the new binary search tree.
     */
    TreeNode * insertNode(TreeNode * root, TreeNode * node) {
        if(!root) return node;
        if(node->val < root->val) root->left=insertNode(root->left,node);
        else root->right=insertNode(root->right,node);
        return root;
    }
};
```