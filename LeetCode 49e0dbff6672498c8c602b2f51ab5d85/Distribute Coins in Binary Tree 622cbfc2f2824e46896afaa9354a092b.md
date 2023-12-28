# Distribute Coins in Binary Tree

#: 979
Difficult: Medium
Tags: DFS, Greedy, Tree
link: https://leetcode.com/problems/distribute-coins-in-binary-tree/description/
Priority: High
Created time: June 27, 2023 11:47 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Binary Tree Cameras (Binary%20Tree%20Cameras%200a1832b41abe4e978c255a5862f62f3d.md)

You are given the `root` of a binary tree with `n` nodes where each `node` in the tree has `node.val` coins. There are `n` coins in total throughout the whole tree.

In one move, we may choose two adjacent nodes and move one coin from one node to another. A move may be from parent to child, or from child to parent.

Return *the **minimum** number of moves required to make every node have **exactly** one coin*.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/01/18/tree1.png](https://assets.leetcode.com/uploads/2019/01/18/tree1.png)

```
Input: root = [3,0,0]
Output: 2
Explanation:From the root of the tree, we move one coin to its left child, and one coin to its right child.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2019/01/18/tree2.png](https://assets.leetcode.com/uploads/2019/01/18/tree2.png)

```
Input: root = [0,3,0]
Output: 3
Explanation:From the left child of the root, we move two coins to the root [taking two moves]. Then, we move one coin from the root of the tree to the right child.

```

**Constraints:**

- The number of nodes in the tree is `n`.
- `1 <= n <= 100`
- `0 <= Node.val <= n`
- The sum of all `Node.val` is `n`.

comment:

有了思路就不难，但是是做了Binary Tree Camera再做的这道题，有点greedy的感觉，直接做不知道能不能想出来做法。

而且实现上有点磕磕绊绊，不太顺畅。

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
    int res=0;

    int distributeCoins(TreeNode* root) {
        helper(root);
        return res;
    }

    //give root, return its val after processing
    int helper(TreeNode* root){
        if(!root) return 1;
        int l=helper(root->left);
        int r=helper(root->right);
        root->val += l-1; res+= abs(l-1);
        root->val += r-1; res+= abs(r-1);
        return root->val;
    }
};

/*
n: 1~100
================================
greedy:
if leave_val != 1, eventually it need one process from parent to give/take coins,
so greedy handle leave first - seq: leave -> root

if leave_val==1, ok
if leave_val==0, take one from parent: parent_val --, step++
if leave_val>1,  give to parent: parent_val+=leave_val-1, step+=leave_val-1

init:
    null -> ok, 0
    leave -> not required if we init null as 0

return f(root);
*/
```