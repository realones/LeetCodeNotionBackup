# Binary Tree Upside Down

#: 156
Difficult: Medium
Tags: DFS, LinkedList, Tree, reverseAll, stack
link: https://leetcode.com/problems/binary-tree-upside-down/
Priority: High
Status: Dig More, More Solution
Created time: October 17, 2022 8:31 PM
Last edited time: October 12, 2023 2:56 PM
source: googleFreq

Given the `root` of a binary tree, turn the tree upside down and return *the new root*.

You can turn a binary tree upside down with the following steps:

1. The original left child becomes the new root.
2. The original root becomes the new right child.
3. The original right child becomes the new left child.

![https://assets.leetcode.com/uploads/2020/08/29/main.jpg](https://assets.leetcode.com/uploads/2020/08/29/main.jpg)

The mentioned steps are done level by level. It is **guaranteed** that every right node has a sibling (a left node with the same parent) and has no children.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/08/29/updown.jpg](https://assets.leetcode.com/uploads/2020/08/29/updown.jpg)

```
Input: root = [1,2,3,4,5]
Output: [4,5,2,null,null,3,1]

```

**Example 2:**

```
Input: root = []
Output: []

```

**Example 3:**

```
Input: root = [1]
Output: [1]

```

**Constraints:**

- The number of nodes in the tree will be in the range `[0, 10]`.
- `1 <= Node.val <= 10`
- Every right node in the tree has a sibling (a left node that shares the same parent).
- Every right node in the tree has no children.

Solution: stk

***remember to cut existing link before add to new tree, otherwise infinite loop

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

/*
tree shape:
    a line from root till left most
    some left node have siblings
*/
class Solution {
public:
    TreeNode* upsideDownBinaryTree(TreeNode* root) {
        TreeNode* dummy = new TreeNode(0);
        TreeNode* pre=dummy;
        stack<TreeNode*> stk;
        pushLeft(stk, root);
        while(!stk.empty()){
            //sequence: root->right most
            TreeNode* cur=stk.top(); stk.pop();
            cur->left=NULL;
            cur->right=NULL;
            //cur left <- sibling(right) if exist
            if(!stk.empty() && stk.top()->right){
                TreeNode* topRight=stk.top()->right;
                cur->left=topRight;
                topRight->left=NULL;
                topRight->right=NULL;
            }
                
            pre->right=cur;
            pre=cur;
        }
        return dummy->right;
    }
    
    void pushLeft(stack<TreeNode*>& stk, TreeNode* node){
        while(node){
            stk.push(node);
            node=node->left;
        }
    }
};
```

Solution: recursive solution from above:

```cpp
class Solution {
public:
    TreeNode* upsideDownBinaryTree(TreeNode* root) {
        if(!root) return NULL;
        TreeNode* l = root->left;
        TreeNode* r = root->right;
        if(!l) return root;//left most as new root
        TreeNode* res=upsideDownBinaryTree(l);
        l->left=r;
        l->right=root;
        root->left=NULL, root->right=NULL;
        return res;
    }
};
```

Solution: top down: root - - - > left most == reverse linked list

```cpp
class Solution {
public:
    TreeNode* upsideDownBinaryTree(TreeNode* root) {
        TreeNode* pre=NULL;
        TreeNode* cur=root;
        TreeNode *nxt=NULL,*RforNxt=NULL;
        while(cur){
            nxt=cur->left;
            
            cur->left=RforNxt;
            RforNxt=cur->right;
            
            cur->right=pre;
            
            pre=cur;
            cur=nxt;
        }
        return pre;
    }
};
```