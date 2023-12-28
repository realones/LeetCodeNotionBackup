# Kth Ancestor of a Tree Node

#: 1483
Difficult: Hard
Tags: BFS, BS_todo, BinaryLifting, Bit Manipulation, DFS, DP, Design, Tree
link: https://leetcode.com/problems/kth-ancestor-of-a-tree-node/description/
Priority: High
Status: More Solution
Created time: June 27, 2023 10:47 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Maximize Value of Function in a Ball Passing Game (Maximize%20Value%20of%20Function%20in%20a%20Ball%20Passing%20Game%20fc4b3d00a1da4f7db13186c996ec3007.md), Lowest Common Ancestor of a Binary Tree (Lowest%20Common%20Ancestor%20of%20a%20Binary%20Tree%20358623c63e324e6e9d5de06b9aa3ec65.md)

You are given a tree with `n` nodes numbered from `0` to `n - 1` in the form of a parent array `parent` where `parent[i]` is the parent of `ith` node. The root of the tree is node `0`. Find the `kth` ancestor of a given node.

The `kth` ancestor of a tree node is the `kth` node in the path from that node to the root node.

Implement the `TreeAncestor` class:

- `TreeAncestor(int n, int[] parent)` Initializes the object with the number of nodes in the tree and the parent array.
- `int getKthAncestor(int node, int k)` return the `kth` ancestor of the given node `node`. If there is no such ancestor, return `1`.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/08/28/1528_ex1.png](https://assets.leetcode.com/uploads/2019/08/28/1528_ex1.png)

```
Input
["TreeAncestor", "getKthAncestor", "getKthAncestor", "getKthAncestor"]
[[7, [-1, 0, 0, 1, 1, 2, 2]], [3, 1], [5, 2], [6, 3]]
Output
[null, 1, 0, -1]

Explanation
TreeAncestor treeAncestor = new TreeAncestor(7, [-1, 0, 0, 1, 1, 2, 2]);
treeAncestor.getKthAncestor(3, 1); // returns 1 which is the parent of 3
treeAncestor.getKthAncestor(5, 2); // returns 0 which is the grandparent of 5
treeAncestor.getKthAncestor(6, 3); // returns -1 because there is no such ancestor
```

**Constraints:**

- `1 <= k <= n <= 5 * 104`
- `parent.length == n`
- `parent[0] == -1`
- `0 <= parent[i] < n` for all `0 < i < n`
- `0 <= node < n`
- There will be at most `5 * 104` queries.

Solution: binary lifting

```cpp
class TreeAncestor {
public:
    vector<vector<int>> dp;
    int M;

    TreeAncestor(int n, vector<int>& parent) {
        M=log2(5e4);
        dp.resize(M+1,vector<int>(n,0));
        //init
        for(int j=0; j<n; j++){
            dp[0][j]=parent[j];
        }
        for(int i=1;i<M+1;i++){
            for(int j=0;j<n;j++){
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
};

/**
 * Your TreeAncestor object will be instantiated and called as such:
 * TreeAncestor* obj = new TreeAncestor(n, parent);
 * int param_1 = obj->getKthAncestor(node,k);
 */

/*

dp[i][j]: i^2 steps up, start from idx j, the target idx 
dp[log2(5e4)][n]

dp[0][j]=parent[j]

dp[i][j]=dp[i-1][j2], j2=dp[i-1][j]

k: 101010010111
for each bit 0->logs(5e4)
    update idx = dp[bit][idx]
*/
```