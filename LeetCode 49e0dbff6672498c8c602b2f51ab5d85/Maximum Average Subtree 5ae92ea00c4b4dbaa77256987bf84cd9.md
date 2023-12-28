# Maximum Average Subtree

#: 1120
Difficult: Medium
Tags: DFS, Tree
link: https://leetcode.com/problems/maximum-average-subtree/description/
Priority: Pass
Created time: December 16, 2023 1:14 AM
Last edited time: December 16, 2023 1:14 AM
source: leetcode_daily

Given the `root` of a binary tree, return *the maximum **average** value of a **subtree** of that tree*. Answers within `10-5` of the actual answer will be accepted.

A **subtree** of a tree is any node of that tree plus all its descendants.

The **average** value of a tree is the sum of its values, divided by the number of nodes.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/04/09/1308_example_1.png](https://assets.leetcode.com/uploads/2019/04/09/1308_example_1.png)

```
Input: root = [5,6,1]
Output: 6.00000
Explanation:
For the node with value = 5 we have an average of (5 + 6 + 1) / 3 = 4.
For the node with value = 6 we have an average of 6 / 1 = 6.
For the node with value = 1 we have an average of 1 / 1 = 1.
So the answer is 6 which is the maximum.

```

**Example 2:**

```
Input: root = [0,null,1]
Output: 1.00000

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `0 <= Node.val <= 105`

```java
//cp from ans to save time
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    double res = Integer.MIN_VALUE;

    public double maximumAverageSubtree(TreeNode root) {
        dfs(root);
        return res;
    }

    private int[] dfs(TreeNode root) {
        if (root == null) {
            return new int[] {0, 0};
        }
        int[] l = dfs(root.left), r = dfs(root.right);
        int sum = l[0] + r[0] + root.val, count = l[1] + r[1] + 1;
        res = Math.max(1.0 * sum / count, res);
        return new int[] {sum, count};
    }
}
```