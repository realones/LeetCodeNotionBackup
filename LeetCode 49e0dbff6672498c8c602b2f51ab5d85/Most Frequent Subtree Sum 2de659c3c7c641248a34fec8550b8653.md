# Most Frequent Subtree Sum

#: 508
Difficult: Medium
Tags: DFS, Hash, Tree
link: https://leetcode.com/problems/most-frequent-subtree-sum/description/
Priority: Low
Created time: August 13, 2023 8:39 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given the `root` of a binary tree, return the most frequent **subtree sum**. If there is a tie, return all the values with the highest frequency in any order.

The **subtree sum** of a node is defined as the sum of all the node values formed by the subtree rooted at that node (including the node itself).

**Example 1:**

![https://assets.leetcode.com/uploads/2021/04/24/freq1-tree.jpg](https://assets.leetcode.com/uploads/2021/04/24/freq1-tree.jpg)

```
Input: root = [5,2,-3]
Output: [2,-3,4]

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/04/24/freq2-tree.jpg](https://assets.leetcode.com/uploads/2021/04/24/freq2-tree.jpg)

```
Input: root = [5,2,-5]
Output: [2]

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `105 <= Node.val <= 105`

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
    vector<int> findFrequentTreeSum(TreeNode* root) {
        unordered_map<int,int> mp;//sum - cnt
        helper(root,mp);
        vector<int> res;
        int maxCnt=0;
        for(auto [sum,cnt]: mp){
            if(cnt<maxCnt){
                continue;
            }
            else if(cnt==maxCnt){
                res.push_back(sum);
            }
            else{
                res={sum};
                maxCnt=cnt;
            }
        }
        return res;
    }

    //find sum of given root, put to mp, and find subtrees...
    int helper(TreeNode* root, unordered_map<int,int>& mp){
        if(!root) return 0;
        int l=helper(root->left,mp);
        int r=helper(root->right,mp);
        int sum=root->val+l+r;
        mp[sum]++;
        return sum;
    }
};

/*
most frequent subtree sum
*
```