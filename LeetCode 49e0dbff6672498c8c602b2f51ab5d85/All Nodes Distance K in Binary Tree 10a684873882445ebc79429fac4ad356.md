# All Nodes Distance K in Binary Tree

#: 863
Difficult: Medium
Tags: BFS, Graph, Tree
link: https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree
Priority: Medium
Status: More Solution
Created time: June 27, 2023 11:48 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, leetcode_daily

Given the `root` of a binary tree, the value of a target node `target`, and an integer `k`, return *an array of the values of all nodes that have a distance* `k` *from the target node.*

You can return the answer in **any order**.

**Example 1:**

![https://s3-lc-upload.s3.amazonaws.com/uploads/2018/06/28/sketch0.png](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/06/28/sketch0.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], target = 5, k = 2
Output: [7,4,1]
Explanation: The nodes that are a distance 2 from the target node (with value 5) have values 7, 4, and 1.

```

**Example 2:**

```
Input: root = [1], target = 1, k = 3
Output: []

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 500]`.
- `0 <= Node.val <= 500`
- All the values `Node.val` are **unique**.
- `target` is the value of one of the nodes in the tree.
- `0 <= k <= 1000`

Solution:

build undirected graph from tree

k time of bfs from target to find k distant nodes from target

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> distanceK(TreeNode* root, TreeNode* target, int k) {
        //build graph
        unordered_map<TreeNode*, unordered_set<TreeNode*>> g;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            TreeNode* cur=q.front(); q.pop();
            if(cur->left){
                g[cur].insert(cur->left);
                g[cur->left].insert(cur);
                q.push(cur->left);
            }
            if(cur->right){
                g[cur].insert(cur->right);
                g[cur->right].insert(cur);
                q.push(cur->right);
            }
        }
        //bfs k times
        vector<int> res;
        unordered_set<TreeNode*> visited;
        visited.insert(target);
        q.push(target);
        while(!q.empty() && k>0){
            int sz=q.size();
            for(int i=0; i<sz; i++){
                TreeNode* cur=q.front(); q.pop();
                for(auto nei: g[cur]){
                    if(visited.count(nei)) continue;
                    visited.insert(nei);
                    q.push(nei);
                }
            }
            k--;
        }
        while(!q.empty()){
            res.push_back(q.front()->val); q.pop();
        }
        return res;
    }
};

/*
tree node:
    cnt: 1~500
    val: 0~500, all unique
target: valid
k: 0~1000

bfs for k steps
visited to avoid go back
from node to children/ parent

implementation:
build graph, 
t:500 O(node cnt)
s:3*500 O(node cnt)

bfs: min of O(node cnt), O(3k)

*/
```