# Number of Good Paths

#: 2421
Difficult: Hard
Tags: DFS, Graph, Tree, Union Find
link: https://leetcode.com/problems/number-of-good-paths/
Priority: High
Status: More Solution
Created time: October 23, 2022 5:40 PM
Last edited time: October 12, 2023 2:56 PM
source: googleFreq

There is a tree (i.e. a connected, undirected graph with no cycles) consisting of `n` nodes numbered from `0` to `n - 1` and exactly `n - 1` edges.

You are given a **0-indexed** integer array `vals` of length `n` where `vals[i]` denotes the value of the `ith` node. You are also given a 2D integer array `edges` where `edges[i] = [ai, bi]` denotes that there exists an **undirected** edge connecting nodes `ai` and `bi`.

A **good path** is a simple path that satisfies the following conditions:

1. The starting node and the ending node have the **same** value.
2. All nodes between the starting node and the ending node have values **less than or equal to** the starting node (i.e. the starting node's value should be the maximum value along the path).

Return *the number of distinct good paths*.

Note that a path and its reverse are counted as the **same** path. For example, `0 -> 1` is considered to be the same as `1 -> 0`. A single node is also considered as a valid path.

**Example 1:**

![https://assets.leetcode.com/uploads/2022/08/04/f9caaac15b383af9115c5586779dec5.png](https://assets.leetcode.com/uploads/2022/08/04/f9caaac15b383af9115c5586779dec5.png)

```
Input: vals = [1,3,2,1,3], edges = [[0,1],[0,2],[2,3],[2,4]]
Output: 6
Explanation: There are 5 good paths consisting of a single node.
There is 1 additional good path: 1 -> 0 -> 2 -> 4.
(The reverse path 4 -> 2 -> 0 -> 1 is treated as the same as 1 -> 0 -> 2 -> 4.)
Note that 0 -> 2 -> 3 is not a good path because vals[2] > vals[0].

```

**Example 2:**

![https://assets.leetcode.com/uploads/2022/08/04/149d3065ec165a71a1b9aec890776ff.png](https://assets.leetcode.com/uploads/2022/08/04/149d3065ec165a71a1b9aec890776ff.png)

```
Input: vals = [1,1,2,2,3], edges = [[0,1],[1,2],[2,3],[2,4]]
Output: 7
Explanation: There are 5 good paths consisting of a single node.
There are 2 additional good paths: 0 -> 1 and 2 -> 3.

```

**Example 3:**

![https://assets.leetcode.com/uploads/2022/08/04/31705e22af3d9c0a557459bc7d1b62d.png](https://assets.leetcode.com/uploads/2022/08/04/31705e22af3d9c0a557459bc7d1b62d.png)

```
Input: vals = [1], edges = []
Output: 1
Explanation: The tree consists of only one node, so there is one good path.

```

**Constraints:**

- `n == vals.length`
- `1 <= n <= 3 * 104`
- `0 <= vals[i] <= 105`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- `edges` represents a valid tree.

Solution Union Find:

```cpp
class Solution {
public:
    vector<int> father;

    void initUnionFind(int n) {
        father.resize(n,0);
        for(int i=0;i<n;i++){
            father[i]=i;//parent is itself
        }
    }

    //with pass compression
    // O(1)
    int find(int a){
        if (father[a] == a) return a;
        father[a] = find(father[a]); //pass compression
        return father[a];
    }

    //Union - Union roots, tree like
    // O(1)
    bool Union(int a, int b) {
        int root_a = find(a);
        int root_b = find(b);
        if (root_a != root_b){
            father[root_a] = root_b;
            return true;
        }
        return false;
    }
    
    int numberOfGoodPaths(vector<int>& vals, vector<vector<int>>& edges) {
        //edges-> mp: l-> {l->s}, improve to be vector later
        initUnionFind(vals.size());//idx based
        
        unordered_map<int, vector<int>> val2idx;
        for(int i=0; i<vals.size(); i++){
            val2idx[vals[i]].push_back(i);
        }
        
        map<int, vector<pair<int,int>>> mp;//l_val->l_idx-s_idx
        for(auto& e: edges){
            int a=e[0], b=e[1];
            if(vals[a] < vals[b]){
                swap(a,b);
            }
            mp[vals[a]].push_back({a,b});
        }
        
        int res=0;
        
        for(auto& [v, e]: mp){//for each val
            for(auto& [a,b]: e){//a large idx, b small idx
                Union(a,b);
            }
            unordered_map<int,int> root_cnt;
            for(int idx: val2idx[v]){//a large idx, b small idx
                root_cnt[find(idx)]++;
            }
            for(auto& [r,cnt]: root_cnt){
                res+=cnt*(cnt-1)/2;
            }
        }
        return res+vals.size();
    }
};
```