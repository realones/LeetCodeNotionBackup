# Maximum Level Sum of a Binary Tree

#: 1161
Difficult: Medium
Tags: BFS, Tree
link: https://leetcode.com/problems/maximum-level-sum-of-a-binary-tree/
Priority: Low
Created time: June 14, 2023 6:13 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_75, leetcode_daily

Given the `root` of a binary tree, the level of its root is `1`, the level of its children is `2`, and so on.

Return the **smallest** level `x` such that the sum of all the values of nodes at level `x` is **maximal**.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/05/03/capture.JPG](https://assets.leetcode.com/uploads/2019/05/03/capture.JPG)

```
Input: root = [1,7,0,7,-8,null,null]
Output: 2
Explanation:
Level 1 sum = 1.
Level 2 sum = 7 + 0 = 7.
Level 3 sum = 7 + -8 = -1.
So we return the level with the maximum sum which is level 2.

```

**Example 2:**

```
Input: root = [989,null,10250,98693,-89388,null,null,null,-32127]
Output: 2

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `105 <= Node.val <= 105`

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
    int maxLevelSum(TreeNode* root) {
        int maxSum=INT_MIN;
        int maxSumlvl=1;
        int lvl=1;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            int sz=q.size();
            int tmp=0;
            for(int i=0; i<sz; i++){
                TreeNode* cur=q.front(); q.pop();
                tmp+=cur->val;
                if(cur->left){q.push(cur->left);}
                if(cur->right){q.push(cur->right);}
            }
            if(tmp>maxSum){
                maxSum=tmp;
                maxSumlvl=lvl;
            }
            lvl++;
        }
        return maxSumlvl;
    }
};

/*
root lvl 1
min lvl that sum of this lvl node vals max
*/
```