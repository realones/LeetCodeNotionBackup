# Average of Levels in Binary Tree

#: 637
Difficult: Easy
Tags: BFS, Tree
link: https://leetcode.com/problems/average-of-levels-in-binary-tree/
Priority: Pass
Created time: June 5, 2023 1:15 AM
Last edited time: November 3, 2023 7:15 PM
practicedTimes: InProgress
source: HuaHua, LC_Top_Interview_150

Given the

```
root
```

of a binary tree, return

*the average value of the nodes on each level in the form of an array*

. Answers within

```
10-5
```

of the actual answer will be accepted.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/03/09/avg1-tree.jpg](https://assets.leetcode.com/uploads/2021/03/09/avg1-tree.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: [3.00000,14.50000,11.00000]
Explanation: The average value of nodes on level 0 is 3, on level 1 is 14.5, and on level 2 is 11.
Hence return [3, 14.5, 11].

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/03/09/avg2-tree.jpg](https://assets.leetcode.com/uploads/2021/03/09/avg2-tree.jpg)

```
Input: root = [3,9,20,15,7]
Output: [3.00000,14.50000,11.00000]

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `231 <= Node.val <= 231 - 1`

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
    vector<double> averageOfLevels(TreeNode* root) {
        vector<double> res;
        if(!root) return res;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            int sz=q.size();
            long long sum=0;
            for(int i=0; i<sz; i++){
                TreeNode* cur=q.front(); q.pop();
                sum+=cur->val;
                if(cur->left){q.push(cur->left);}
                if(cur->right){q.push(cur->right);}
            }
            res.push_back(sum*1.0/sz); //process
        }
        return res;
    }
};
```