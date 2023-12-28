# Path Sum III

#: 437
Difficult: Medium
Tags: BackTracking, DFS, Hash, Prefix, Tree
link: https://leetcode.com/problems/path-sum-iii
Priority: High
Status: HardToImplement
Created time: July 8, 2023 3:41 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_75

Given the `root` of a binary tree and an integer `targetSum`, return *the number of paths where the sum of the values along the path equals* `targetSum`.

The path does not need to start or end at the root or a leaf, but it must go downwards (i.e., traveling only from parent nodes to child nodes).

**Example 1:**

![https://assets.leetcode.com/uploads/2021/04/09/pathsum3-1-tree.jpg](https://assets.leetcode.com/uploads/2021/04/09/pathsum3-1-tree.jpg)

```
Input: root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
Output: 3
Explanation: The paths that sum to 8 are shown.

```

**Example 2:**

```
Input: root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
Output: 3

```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 1000]`.
- `109 <= Node.val <= 109`
- `1000 <= targetSum <= 1000`

Solution 1:

prefix sum mp<prefixSum,cnt> + backtracking

思路转化为代码过程要从高到低一层层的来，先写大框架，再丰富细节实现

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
using ll=long long;

class Solution {
public:
    int res=0;
    int pathSum(TreeNode* root, int targetSum) {
        unordered_map<ll,int> mp;//prefixSum,cnt
        mp[0]=1;

        dfs(root,mp,targetSum,0);
        return res;
    }

    void dfs(TreeNode* node, unordered_map<ll,int>& mp, int targetSum, ll curSum){
        if(!node) return;

        curSum+=node->val;
        if(mp.count(curSum-targetSum)){
            res+=mp[curSum-targetSum];
        }

        mp[curSum]++;
        dfs(node->left,mp,targetSum,curSum);
        dfs(node->right,mp,targetSum,curSum);
        mp[curSum]--;
    }
};

/*
find sumlr: prefixSum
find if exist: hash
find cnt: mp<prefixSum,cnt>
    so from arr start to end, if mp[curSum-targetSum] exist, res+=mp[...]
it is a tree, so use dfs to build prefixsum mp recursively - backtracking
*
```

Solution 2:

brute force

pure recursion, relative difficult to figure out, need to dig into the algorithm

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
using ll=long long;
class Solution {
public:
    //start from root, or l/r
    int pathSum(TreeNode* root, int targetSum) {
        if(!root) return 0;
        return helper(root, targetSum) + pathSum(root->left, targetSum) + pathSum(root->right, targetSum);
    }

    //start from root, end anywhere, sum=target
    ll helper(TreeNode* root, ll targetSum) {
        //question? if(!root && targetSum==0) return 1;
        if(!root) return 0;
        ll res=0;
        if(root->val==targetSum) res++;
        ll l=helper(root->left, targetSum-root->val);
        ll r=helper(root->right, targetSum-root->val);
        res+=l+r;
        return res;
    }
};

/*
if !root return 0
[not required] if leave && val==t return 1

if val==t res+1
res+l
res+r

*/
```