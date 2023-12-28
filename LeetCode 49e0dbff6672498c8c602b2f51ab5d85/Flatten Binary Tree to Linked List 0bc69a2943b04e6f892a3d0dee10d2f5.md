# Flatten Binary Tree to Linked List

#: 114
Difficult: Medium
Tags: Iterate, Tree, changeAffectIteration
link: https://leetcode.com/problems/flatten-binary-tree-to-linked-list/
Priority: High
Created time: July 8, 2022 4:26 PM
Last edited time: October 14, 2023 10:26 PM
practicedTimes: 1
source: LC_Top_Interview_150, jiuzhang, 算初Tree Divide&Conquer

Given the `root` of a binary tree, flatten the tree into a "linked list":

- The "linked list" should use the same `TreeNode` class where the `right` child pointer points to the next node in the list and the `left` child pointer is always `null`.
- The "linked list" should be in the same order as a **[pre-order traversal](https://en.wikipedia.org/wiki/Tree_traversal#Pre-order,_NLR)** of the binary tree.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/01/14/flaten.jpg](https://assets.leetcode.com/uploads/2021/01/14/flaten.jpg)

```
Input: root = [1,2,5,3,4,null,6]
Output: [1,null,2,null,3,null,4,null,5,null,6]

```

**Example 2:**

```
Input: root = []
Output: []

```

**Example 3:**

```
Input: root = [0]
Output: [0]

```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 2000]`.
- `100 <= Node.val <= 100`

**Follow up:**

Can you flatten the tree in-place (with

```
O(1)
```

extra space)?

solution - tpre:

```cpp
class Solution {
public:
    TreeNode* last=NULL;
    void flatten(TreeNode* root) {
        if(!root) return;
        stack<TreeNode*> stk;
        stk.push(root);
        while(!stk.empty()){
            TreeNode* cur=stk.top();stk.pop();
            if(cur->right) stk.push(cur->right);
            if(cur->left) stk.push(cur->left);
            if(last) {
                last->left=NULL;
                last->right=cur;
            }
            last=cur;
        }
    }
};
```

Solution 3: 

Tree结构改变，右 → 左 → 头 traverse 可以保证结构改变的同时不影响遍历顺序

每次让cur node指向last node即可

/**old
Public: first touched node - flatten right then left, use the last touched node to point to it
反过来想，reverse pre order handling: r, l, root
//the current.right link to lastNode (so need to maintain lastNode)

right → left → root, then will keep the order

last is the root, so for flatten(root→right), last will be root→right

in flatten(root→left), the first execution without more recursion is root→left’s bottom right element.

*/

```cpp
class Solution {
public:
    TreeNode* last=NULL;
    void flatten(TreeNode* root) {
        if(!root) return;
        flatten(root->right);
        flatten(root->left);
        root->left=NULL;
        root->right=last;
        last=root;
    }
};
```