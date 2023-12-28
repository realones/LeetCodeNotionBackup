# Longest ZigZag Path in a Binary Tree

#: 1372
Difficult: Medium
Tags: DFS, DP, Tree
link: https://leetcode.com/problems/longest-zigzag-path-in-a-binary-tree/
Priority: Medium
Status: HardToImplement
Created time: July 8, 2023 4:14 PM
Last edited time: November 19, 2023 11:31 PM
source: LC_75

You are given the `root` of a binary tree.

A ZigZag path for a binary tree is defined as follow:

- Choose **any** node in the binary tree and a direction (right or left).
- If the current direction is right, move to the right child of the current node; otherwise, move to the left child.
- Change the direction from right to left or from left to right.
- Repeat the second and third steps until you can't move in the tree.

Zigzag length is defined as the number of nodes visited - 1. (A single node has a length of 0).

Return *the longest **ZigZag** path contained in that tree*.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/01/22/sample_1_1702.png](https://assets.leetcode.com/uploads/2020/01/22/sample_1_1702.png)

```
Input: root = [1,null,1,1,1,null,null,1,1,null,1,null,null,null,1]
Output: 3
Explanation: Longest ZigZag path in blue nodes (right -> left -> right).

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/01/22/sample_2_1702.png](https://assets.leetcode.com/uploads/2020/01/22/sample_2_1702.png)

```
Input: root = [1,1,1,null,1,null,null,1,1,null,1]
Output: 4
Explanation: Longest ZigZag path in blue nodes (left -> right -> left -> right).

```

**Example 3:**

```
Input: root = [1]
Output: 0

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 5 * 104]`.
- `1 <= Node.val <= 100`

todo: from tag i see dp solution

Solution new:

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
    int longestZigZag(TreeNode* root) {
        if(!root) return 0;
        helper(root, 0, 0);
        return res;
    }
    //cnt end with go left/ right
    void helper(TreeNode* node, int lcnt, int rcnt){
        if(!node) return;
        res=max({res, lcnt, rcnt});
        helper(node->left, rcnt+1, 0);
        helper(node->right, 0, lcnt+1);
    }
};

/*
maintain global res
dfs up down to update the res
from parent, we know cnt end with l/r
*/
```

Solution - dfs:

implemented w/o help and pretty fast, but still feel some blocks from thought to code

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
    int L=0, R=1;
    int res=0;
    int longestZigZag(TreeNode* root) {//num: 1~5e4
        dfs(root,L,0);
        dfs(root,R,0);
        return res-1;
    }

    void dfs(TreeNode* node, int preDir, int preLen){
        if(!node) return;

        res=max({res, preLen+1,1});

        if(preDir==L){
            dfs(node->right, R, preLen+1);
            dfs(node->left, L, 1);
        }
        else{//preDir==R
            dfs(node->left, L, preLen+1);
            dfs(node->right, R, 1);
        }
    }
};

/*
node val doesn't matter
dfs find max res by checking 
    start from previous, with opposite direction from previous node, pre+1
    start from cur node, with diff direction with above 0+1 (root can l & r dir)
seems top down dfs is easier than bot up recursion, since bottom-up need child (left+right) * (L+R) = 4 children status?
*
```