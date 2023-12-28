# Delete Nodes And Return Forest

#: 1110
Difficult: Medium
Tags: Array, BFS, DFS, Hash, Tree
link: https://leetcode.com/problems/delete-nodes-and-return-forest/description/
Priority: Medium
Status: More Solution
Created time: June 27, 2023 11:47 PM
Last edited time: December 8, 2023 6:04 PM
source: HuaHua, facebookFreq

Given the `root` of a binary tree, each node in the tree has a distinct value.

After deleting all nodes with a value in `to_delete`, we are left with a forest (a disjoint union of trees).

Return the roots of the trees in the remaining forest. You may return the result in any order.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/07/01/screen-shot-2019-07-01-at-53836-pm.png](https://assets.leetcode.com/uploads/2019/07/01/screen-shot-2019-07-01-at-53836-pm.png)

```
Input: root = [1,2,3,4,5,6,7], to_delete = [3,5]
Output: [[1,2,null,4],[6],[7]]

```

**Example 2:**

```
Input: root = [1,2,4,null,3], to_delete = [3]
Output: [[1,2,4]]

```

**Constraints:**

- The number of nodes in the given tree is at most `1000`.
- Each node has a distinct value between `1` and `1000`.
- `to_delete.length <= 1000`
- `to_delete` contains distinct values between `1` and `1000`.

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
    vector<TreeNode*> delNodes(TreeNode* root, vector<int>& to_delete) {
        unordered_set<int> st(to_delete.begin(), to_delete.end());
        vector<TreeNode*> res;
        if(!root) return res;
        if(!st.count(root->val)) res.push_back(root);

        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            TreeNode* cur=q.front(); q.pop();
            if(st.count(cur->val)){
                if(cur->left && !st.count(cur->left->val)) res.push_back(cur->left);
                if(cur->right && !st.count(cur->right->val)) res.push_back(cur->right);
                //cut
                cur->val=-1;
            }
            if(cur->left){q.push(cur->left);}
            if(cur->right){q.push(cur->right);}
        }
        //cut
        q.push(root);
        while(!q.empty()){
            TreeNode* cur=q.front(); q.pop();
            if(cur->left){
                q.push(cur->left);
                if(cur->left->val==-1) cur->left=NULL;
            }
            if(cur->right){
                q.push(cur->right);
                if(cur->right->val==-1) cur->right=NULL;
            }
        }

        return res;
    
    }
};

/*

================================
node
    cnt: 1000
    val: 1~1000, distinct
to_delete
    len: <=1000
    val: 1~1000, distinct
================================
traverse nodes (dfs, bfs)
    delete node -> use two children as new roots

if tree has two child -> checking l and r
if child can also be removed -> checking in hash_set of to_delete

*/
```