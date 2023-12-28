# Count Unreachable Pairs of Nodes in an Undirected Graph

#: 2316
Difficult: Medium
Tags: BFS, DFS, Graph, Union Find
link: https://leetcode.com/problems/count-unreachable-pairs-of-nodes-in-an-undirected-graph/description/
Priority: Low
Status: More Solution
Created time: June 28, 2023 12:21 AM
Last edited time: December 23, 2023 11:49 PM
source: HuaHua, googleFreq

You are given an integer `n`. There is an **undirected** graph with `n` nodes, numbered from `0` to `n - 1`. You are given a 2D integer array `edges` where `edges[i] = [ai, bi]` denotes that there exists an **undirected** edge connecting nodes `ai` and `bi`.

Return *the **number of pairs** of different nodes that are **unreachable** from each other*.

**Example 1:**

![https://assets.leetcode.com/uploads/2022/05/05/tc-3.png](https://assets.leetcode.com/uploads/2022/05/05/tc-3.png)

```
Input: n = 3, edges = [[0,1],[0,2],[1,2]]
Output: 0
Explanation: There are no pairs of nodes that are unreachable from each other. Therefore, we return 0.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2022/05/05/tc-2.png](https://assets.leetcode.com/uploads/2022/05/05/tc-2.png)

```
Input: n = 7, edges = [[0,2],[0,5],[2,4],[1,6],[5,4]]
Output: 14
Explanation: There are 14 pairs of nodes that are unreachable from each other:
[[0,1],[0,3],[0,6],[1,2],[1,3],[1,4],[1,5],[2,3],[2,6],[3,4],[3,5],[3,6],[4,6],[5,6]].
Therefore, we return 14.

```

**Constraints:**

- `1 <= n <= 105`
- `0 <= edges.length <= 2 * 105`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- There are no repeated edges.

```cpp
//cp from editorial to pass
class UnionFind {
private:
    vector<int> parent, rank;

public:
    UnionFind(int size) {
        parent.resize(size);
        rank.resize(size, 0);
        for (int i = 0; i < size; i++) {
            parent[i] = i;
        }
    }
    int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]);
        return parent[x];
    }
    void union_set(int x, int y) {
        int xset = find(x), yset = find(y);
        if (xset == yset) {
            return;
        } else if (rank[xset] < rank[yset]) {
            parent[xset] = yset;
        } else if (rank[xset] > rank[yset]) {
            parent[yset] = xset;
        } else {
            parent[yset] = xset;
            rank[xset]++;
        }
    }
};

class Solution {
public:
    long long countPairs(int n, vector<vector<int>>& edges) {
        UnionFind dsu(n);
        for (auto edge : edges) {
            dsu.union_set(edge[0], edge[1]);
        }
        unordered_map<int, int> componentSize;
        for (int i = 0; i < n; i++) {
            componentSize[dsu.find(i)]++;
        }

        long long numberOfPaths = 0;
        long long remainingNodes = n;
        for (auto component : componentSize) {
            int componentSize = component.second;
            numberOfPaths += componentSize * (remainingNodes - componentSize);
            remainingNodes -= componentSize;
        }
        return numberOfPaths;
    }
};
/*
union find to find the size of each component
res = sz1*restSz + sz2*restSz + .....
*/
```