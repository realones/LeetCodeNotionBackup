# Delete Leaves With a Given Value

#: 1325
Difficult: Medium
Tags: DFS, Tree
link: https://leetcode.com/problems/delete-leaves-with-a-given-value/description/
Priority: Medium
Created time: August 12, 2023 5:25 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Binary Tree Pruning (Binary%20Tree%20Pruning%2097e144731f5b47f986202cdc95407576.md)

Given a binary tree `root` and an integer `target`, delete all the **leaf nodes** with value `target`.

Note that once you delete a leaf node with value `target`**,** if its parent node becomes a leaf node and has the value `target`, it should also be deleted (you need to continue doing that until you cannot).

**Example 1:**

![https://assets.leetcode.com/uploads/2020/01/09/sample_1_1684.png](https://assets.leetcode.com/uploads/2020/01/09/sample_1_1684.png)

```
Input: root = [1,2,3,2,null,2,4], target = 2
Output: [1,null,3,null,4]
Explanation: Leaf nodes in green with value (target = 2) are removed (Picture in left).
After removing, new nodes become leaf nodes with value (target = 2) (Picture in center).

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/01/09/sample_2_1684.png](https://assets.leetcode.com/uploads/2020/01/09/sample_2_1684.png)

```
Input: root = [1,3,3,3,2], target = 3
Output: [1,3,null,null,2]

```

**Example 3:**

![https://assets.leetcode.com/uploads/2020/01/15/sample_3_1684.png](https://assets.leetcode.com/uploads/2020/01/15/sample_3_1684.png)

```
Input: root = [1,2,null,2,null,2], target = 2
Output: [1]
Explanation: Leaf nodes in green with value (target = 2) are removed at each step.

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 3000]`.
- `1 <= Node.val, target <= 1000`

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
    TreeNode* removeLeafNodes(TreeNode* root, int target) {
        if(!root) return NULL;
        TreeNode* l=removeLeafNodes(root->left, target);
        if(!l) root->left=NULL;
        TreeNode* r=removeLeafNodes(root->right, target);
        if(!r) root->right=NULL;
        if(!root->left && !root->right && root->val==target) return NULL;
        return root;
    }
};
/*
for root
if rl is leave && ==target(invalid), rl=null
if rr is leave && ==target(invalid), rr=null
if root is leave now &&==target, return null

let null==invalid

*/
```