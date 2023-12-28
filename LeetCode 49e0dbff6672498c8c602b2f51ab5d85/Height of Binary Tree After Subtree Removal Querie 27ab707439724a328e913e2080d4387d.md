# Height of Binary Tree After Subtree Removal Queries

#: 2458
Difficult: Hard
Tags: Array, BFS, DFS, Tree
link: https://leetcode.com/problems/height-of-binary-tree-after-subtree-removal-queries/description/
Priority: High
Status: HardToImplement, No Idea At First, checkBetterSolution
Created time: December 16, 2023 10:41 AM
Last edited time: December 16, 2023 10:43 AM
source: googleFreq

You are given the `root` of a **binary tree** with `n` nodes. Each node is assigned a unique value from `1` to `n`. You are also given an array `queries` of size `m`.

You have to perform `m` **independent** queries on the tree where in the `ith` query you do the following:

- **Remove** the subtree rooted at the node with the value `queries[i]` from the tree. It is **guaranteed** that `queries[i]` will **not** be equal to the value of the root.

Return *an array* `answer` *of size* `m` *where* `answer[i]` *is the height of the tree after performing the* `ith` *query*.

**Note**:

- The queries are independent, so the tree returns to its **initial** state after each query.
- The height of a tree is the **number of edges in the longest simple path** from the root to some node in the tree.

**Example 1:**

![https://assets.leetcode.com/uploads/2022/09/07/binaryytreeedrawio-1.png](https://assets.leetcode.com/uploads/2022/09/07/binaryytreeedrawio-1.png)

```
Input: root = [1,3,4,2,null,6,5,null,null,null,null,null,7], queries = [4]
Output: [2]
Explanation: The diagram above shows the tree after removing the subtree rooted at node with value 4.
The height of the tree is 2 (The path 1 -> 3 -> 2).

```

**Example 2:**

![https://assets.leetcode.com/uploads/2022/09/07/binaryytreeedrawio-2.png](https://assets.leetcode.com/uploads/2022/09/07/binaryytreeedrawio-2.png)

```
Input: root = [5,8,9,2,1,3,7,4,6], queries = [3,2,4,8]
Output: [3,2,3,2]
Explanation: We have the following queries:
- Removing the subtree rooted at node with value 3. The height of the tree becomes 3 (The path 5 -> 8 -> 2 -> 4).
- Removing the subtree rooted at node with value 2. The height of the tree becomes 2 (The path 5 -> 8 -> 1).
- Removing the subtree rooted at node with value 4. The height of the tree becomes 3 (The path 5 -> 8 -> 2 -> 6).
- Removing the subtree rooted at node with value 8. The height of the tree becomes 2 (The path 5 -> 9 -> 3).

```

**Constraints:**

- The number of nodes in the tree is `n`.
- `2 <= n <= 105`
- `1 <= Node.val <= n`
- All the values in the tree are **unique**.
- `m == queries.length`
- `1 <= m <= min(n, 104)`
- `1 <= queries[i] <= n`
- `queries[i] != root.val`

comment:

思路思考时间不久，但是实现非常hardtoimplement，感觉总是lost自己在做什么

beat only 5%， check better solution

而且pq不能用lambda很不爽，不如直接先写sort的solution，查查怎么改进

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
    unordered_map<TreeNode*, int> nodeHeight;
class Solution {
public:
    //get node->depth, node->height
    unordered_map<int, unordered_set<TreeNode*>> depthNodes;
    unordered_map<TreeNode*, int> nodeDepth;
    unordered_map<int, TreeNode*> valNode;
    int maxDepth=0;
    int helper(TreeNode* node, int depth=0){//return height
        if(!node) return 0;
        valNode[node->val] = node;
        depthNodes[depth].insert(node);
        maxDepth=max(maxDepth,depth);
        nodeDepth[node]=depth;
        int l=helper(node->left, depth+1);
        int r=helper(node->right, depth+1);
        int h=max(l,r)+1;
        nodeHeight[node]=h;
        return h;
    }

    class MyComp{
    public:
        bool operator()(TreeNode* &a, TreeNode* &b) const{
            return nodeHeight[a]>nodeHeight[b];
        }
    };
    using pq=priority_queue<TreeNode*,vector<TreeNode*>, MyComp>;
    unordered_map<int, vector<TreeNode*>> depthLargestHeight2Nodes;
    void getDepthLargestHeight2Nodes(){
        for(int d=0;d<=maxDepth;d++){
            unordered_set<TreeNode*> nodes = depthNodes[d];
            pq q;
            for(auto node: nodes){
                int height=nodeHeight[node];
                q.push(node);
                if(q.size()>2) q.pop();
            }
            vector<TreeNode*> v;
            while(!q.empty()){
                v.push_back(q.top()); q.pop();
            }
            depthLargestHeight2Nodes[d]=v;
        }
    }

    // T: O(n+m)
    // S: O(n)
    vector<int> treeQueries(TreeNode* root, vector<int>& queries) {
        //preprocess each node
        // we know depth->nodes t:n s:n
        // we know node->depth t:n,s:n
        // we know node->height t:n s:n
        helper(root,0);
        // improve: get largest two node heights, t:n
        getDepthLargestHeight2Nodes();
        // for each query O(m)
        vector<int> res;
        for(auto& q: queries){
            TreeNode* node=valNode[q];
            int d=nodeDepth[node];
            int cur=0;
            vector<TreeNode*> v=depthLargestHeight2Nodes[d];//not use &
            if(node != v.back()){
                cur=maxDepth;
            }
            else{
                if(v.size()==1) {
                    cur=d-1;//this depth layer only has 1 node
                }
                else {
                    cur=d+nodeHeight[v.front()]-1;
                }
            } 
            res.push_back(cur);
        }
        return res;
    }
};

/*
solution1: bruteforce
    for each query t:m
        dfs to find node parent t:n
        rm the node t:1
        dfs to find the height t:n
    T: O(m*n)
    S: O(1)

solution2: 
    preprocess each node
        if node decide max height, or what is the height after it's removing: t:n? s:n?

    bfs to know depth of each layer t:n s:n
        height 0: 1
        height 1: 3 4
        height 2: 2 6 5
        height 2: 7
        
        we know node->depth (reverse link) t:n,s:n
        we know height of each node subtree t:n s:n
            thought1:sort each layer by height, overall t:nlogn
            improve: get largest two node heights, t:n
    
    for each query O(m)
        check the height t:layerCnt=(n/2)? after using sort/improve t:1
    T: O(n+m)
    S: O(n)

    btw, we know all queries in advance, maybe we can do something like sort? may be in solution 3?
*/
```