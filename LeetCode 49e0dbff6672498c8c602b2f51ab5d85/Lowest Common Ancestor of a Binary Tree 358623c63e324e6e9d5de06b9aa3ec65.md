# Lowest Common Ancestor of a Binary Tree

#: 236
Difficult: Medium
Tags: BinaryLifting, DFS, Divide and Conquer, Tree
link: https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/
Priority: High
Status: Dig More, toRevisit
Created time: July 8, 2022 4:26 PM
Last edited time: October 17, 2023 6:24 PM
source: HuaHua, LC_75, LC_Top_Interview_150, jiuzhang, 算初Tree Divide&Conquer
related: Kth Ancestor of a Tree Node (Kth%20Ancestor%20of%20a%20Tree%20Node%20847479db64aa4644aafb1eb95986bdcf.md), Minimum Edge Weight Equilibrium Queries in a Tree (Minimum%20Edge%20Weight%20Equilibrium%20Queries%20in%20a%20Tree%20ba9b8fbaf4f84f88b3432babdc3bf0fb.md), Lowest Common Ancestor of a Binary Tree II (Lowest%20Common%20Ancestor%20of%20a%20Binary%20Tree%20II%20d25a677fff6f4935bac0a9c113aa1098.md)

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).”

**Example 1:**

![https://assets.leetcode.com/uploads/2018/12/14/binarytree.png](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2018/12/14/binarytree.png](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.

```

**Example 3:**

```
Input: root = [1,2], p = 1, q = 2
Output: 1

```

**Constraints:**

- The number of nodes in the tree is in the range `[2, 105]`.
- `109 <= Node.val <= 109`
- All `Node.val` are **unique**.
- `p != q`
- `p` and `q` will exist in the tree.

solution binary lifting:

the last part to find lca is tricky, like reversely find the largest k step by scanning every bit from large to small, that < steps to the lca

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
    //this is code from "Kth Ancestor of a Tree Node"
    vector<unordered_map<int, int>> dp;//2^i step up, j idx -- target idx
    int M;
    unordered_map<int, int> depth;
    unordered_map<int, int> parent;
    unordered_map<int, TreeNode*> val_node;
    int n;//num of node

    void initTreeAncestor() {
        M=log2(n);
        dp.resize(M+1);
        //init
        for(auto [j,_]: val_node){
            dp[0][j]=parent[j];
        }

        for(int i=1;i<M+1;i++){
            for(auto [j,_]: val_node){
                int j2=dp[i-1][j];
                dp[i][j] = j2==-1?-1:dp[i-1][j2];
            }
        }
    }
    
    int getKthAncestor(int node, int k) {
        for(int i=0; i<M+1; i++){
            if(k & (1<<i)){
                if(node==-1) return -1;
                node=dp[i][node];
            }
        }
        return node;
    }
    
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        //build node val set, since node val is unique
        //get depth of p and q
        //build parent dependency mp[node, parent]
        dfs(root,-1,0);
        //init k ancestor dp
        n=val_node.size();

        initTreeAncestor();
        //ensure both nodes are on the same level
        int node1=p->val, node2=q->val;
        if(depth[node1] > depth[node2]){
            node1=getKthAncestor(node1, depth[node1]-depth[node2]);
        }
        else if(depth[node1] < depth[node2]){
            node2=getKthAncestor(node2, depth[node2]-depth[node1]);
        }
        if(node1==node2) return val_node[node1];
        //jump from large to small steps to find the lca
        for(int i=M; i>=0; i--){//2^i steps up
            int node12=dp[i][node1];
            int node22=dp[i][node2];
            if(node12==node22) continue;
            else{
                node1=node12;
                node2=node22;
            }
        }
        return val_node[parent[node1]];
    }

    void dfs(TreeNode* node, int parentVal, int dep){
        if(!node) return;
        val_node[node->val]=node;
        parent[node->val]=parentVal;
        depth[node->val]=dep;
        dfs(node->left, node->val, dep+1);
        dfs(node->right, node->val, dep+1);
    }
};

/*
for following to to m times, that m*n is huge e.g. 1e10

https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/solutions/1636280/java-a-summary-on-binary-lifting-to-find-lca/

*binary lifting*
step 1: build kthAncestor(node, k) similar to "Kth Ancestor of a Tree Node"
    O(nlogn) to prepare dp
    call this function after dp preparation is O(logk), can treat as O(logn) as worst
step 2: Ensure both nodes are on the same level.
    kthAncestor(node, levelDifference) on lower node to same level O(logn)
step 3: 
    simlar to BS
        if jump to or over LCA, they jumped to same node,
        else, not jumped enough
    so the algorithm:
        jump largest possible step: 2^log(n) (it will be larger than n/2 anyway)
            if overshoot, don't make the jump
            else, jump
        next, jump step: 2^(log(n)-1)
        next, jump step: 2^(log(n)-2)
        next, jump step: 2^(log(n)-3)
        ...
        finaly, jump step 2^0
    in the end, the direct parent of any nodes is LCA
*/
```

[imaging: 从下往上染色，A一种颜色，B一种颜色，A+B一种颜色]

return not_found/p/q/ancestor
follow up: LCA II & III

```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
		    if (!root || root == p || root == q) return root;
		    TreeNode* left = lowestCommonAncestor(root->left, p, q);
		    TreeNode* right = lowestCommonAncestor(root->right, p, q);
		    if(left && right) return root;
		    if(left) return left;
		    if(right) return right;
		    return NULL;
		}
};
```