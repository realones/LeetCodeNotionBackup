# Count Nodes Equal to Average of Subtree

#: 2265
Difficult: Medium
Tags: DFS, Tree
link: https://leetcode.com/problems/count-nodes-equal-to-average-of-subtree/
Priority: Pass
Created time: November 1, 2023 8:54 PM
Last edited time: November 1, 2023 8:54 PM
source: leetcode_daily

Given the `root` of a binary tree, return *the number of nodes where the value of the node is equal to the **average** of the values in its **subtree***.

**Note:**

- The **average** of `n` elements is the **sum** of the `n` elements divided by `n` and **rounded down** to the nearest integer.
- A **subtree** of `root` is a tree consisting of `root` and all of its descendants.

**Example 1:**

![https://assets.leetcode.com/uploads/2022/03/15/image-20220315203925-1.png](https://assets.leetcode.com/uploads/2022/03/15/image-20220315203925-1.png)

```
Input: root = [4,8,5,0,1,null,6]
Output: 5
Explanation:
For the node with value 4: The average of its subtree is (4 + 8 + 5 + 0 + 1 + 6) / 6 = 24 / 6 = 4.
For the node with value 5: The average of its subtree is (5 + 6) / 2 = 11 / 2 = 5.
For the node with value 0: The average of its subtree is 0 / 1 = 0.
For the node with value 1: The average of its subtree is 1 / 1 = 1.
For the node with value 6: The average of its subtree is 6 / 1 = 6.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2022/03/26/image-20220326133920-1.png](https://assets.leetcode.com/uploads/2022/03/26/image-20220326133920-1.png)

```
Input: root = [1]
Output: 1
Explanation: For the node with value 1: The average of its subtree is 1 / 1 = 1.

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 1000]`.
- `0 <= Node.val <= 1000`

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
using ii=pair<int,int>;

class Solution {
public:
    int res=0;

    int averageOfSubtree(TreeNode* root) {
        helper(root);
        return res;
    }

    //return (cnt,sum)
    ii helper(TreeNode* root){
        if(!root) return {0,0};
        auto [lcnt,lsum] = helper(root->left);
        auto [rcnt,rsum] = helper(root->right);
        int cnt=lcnt+rcnt+1;
        int sum=lsum+rsum+root->val;
        if(root->val == sum/cnt) res++;
        return {cnt, sum};
    }
};
```