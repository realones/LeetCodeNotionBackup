# Find Duplicate Subtrees

#: 652
Difficult: Medium
Tags: DFS, Hash, Tree
link: https://leetcode.com/problems/find-duplicate-subtrees/description/
Priority: Medium
Status: More Solution
Created time: June 27, 2023 11:49 PM
Last edited time: December 24, 2023 1:38 AM
source: HuaHua, googleFreq

Given the `root` of a binary tree, return all **duplicate subtrees**.

For each kind of duplicate subtrees, you only need to return the root node of any **one** of them.

Two trees are **duplicate** if they have the **same structure** with the **same node values**.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/08/16/e1.jpg](https://assets.leetcode.com/uploads/2020/08/16/e1.jpg)

```
Input: root = [1,2,3,4,null,2,4,null,null,4]
Output: [[2,4],[4]]

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/08/16/e2.jpg](https://assets.leetcode.com/uploads/2020/08/16/e2.jpg)

```
Input: root = [2,1,1]
Output: [[1]]

```

**Example 3:**

![https://assets.leetcode.com/uploads/2020/08/16/e33.jpg](https://assets.leetcode.com/uploads/2020/08/16/e33.jpg)

```
Input: root = [2,2,2,3,null,3,null]
Output: [[2,3],[3]]

```

**Constraints:**

- The number of the nodes in the tree will be in the range `[1, 5000]`
- `200 <= Node.val <= 200`

comment: 这个取值范围，居然没有TLE

check more solutions from Editorial - 2

```cpp
//cp from Editorial - 1

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
    vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
        unordered_map<string, int> cnt;
        vector<TreeNode*> res;
        function<string(TreeNode*)> traverse = [&cnt, &res, &traverse](TreeNode* node) -> string {
            if (node == nullptr) {
                return "";
            }
            string representation = "(" + traverse(node->left) + ")" + to_string(node->val) + "(" +
                                    traverse(node->right) + ")";
            cnt[representation]++;
            if (cnt[representation] == 2) {
                res.push_back(node);
            }
            return representation;
        };
        traverse(root);
        return res;
    }
};

/*
serialize and compare

================================
node cnt: 1~5000
brute force:
    n*n * n

mp: node - serialization, compare
    5000 * 5000

observation:
    same height: mp[node, height]
    same root->val
    find largest same tree, then all subnodes add to res
    mark as visited
*/
```