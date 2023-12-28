# Redundant Connection II

#: 685
Difficult: Hard
Tags: Graph, TopSorting, Tree, Union Find
link: https://leetcode.com/problems/redundant-connection-ii/
Priority: High
Status: No Idea At First
Created time: June 27, 2023 11:50 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

In this problem, a rooted tree is a **directed** graph such that, there is exactly one node (the root) for which all other nodes are descendants of this node, plus every node has exactly one parent, except for the root node which has no parents.

The given input is a directed graph that started as a rooted tree with `n` nodes (with distinct values from `1` to `n`), with one additional directed edge added. The added edge has two different vertices chosen from `1` to `n`, and was not an edge that already existed.

The resulting graph is given as a 2D-array of `edges`. Each element of `edges` is a pair `[ui, vi]` that represents a **directed** edge connecting nodes `ui` and `vi`, where `ui` is a parent of child `vi`.

Return *an edge that can be removed so that the resulting graph is a rooted tree of* `n` *nodes*. If there are multiple answers, return the answer that occurs last in the given 2D-array.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/12/20/graph1.jpg](https://assets.leetcode.com/uploads/2020/12/20/graph1.jpg)

```
Input: edges = [[1,2],[1,3],[2,3]]
Output: [2,3]

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/12/20/graph2.jpg](https://assets.leetcode.com/uploads/2020/12/20/graph2.jpg)

```
Input: edges = [[1,2],[2,3],[3,4],[4,1],[1,5]]
Output: [4,1]

```

**Constraints:**

- `n == edges.length`
- `3 <= n <= 1000`
- `edges[i].length == 2`
- `1 <= ui, vi <= n`
- `ui != vi`

comment: after completed “****Redundant Connection****”, still not able to figure it out, just list ind and outd in a spreadsheet would definitely help.

Solution union_find

```cpp
class Solution {
public:
    //init
    vector<int> father;//use hash_map<type,type> if not using idx as identifier
    void init(int n) {//not required if using hash_map, init in begining of find()
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
    
    //union - union roots, tree like
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

    vector<int> findRedundantDirectedConnectionI(vector<vector<int>>& edges, vector<int> skip) {
        //find circle
        int n=edges.size();
        init(n+1);

        for(auto& e: edges){
            int a=e[0], b=e[1];
            if(a==skip[0] && b==skip[1]) continue;
            if(Union(a,b)) continue;
            else return {a,b};
        }
        return {-1,-1};
    }

    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        int n=edges.size();
        unordered_map<int, int> parent;
        vector<int>candA,candB;
        for(auto& e: edges){
            int a=e[0], b=e[1];
            if(parent.count(b)){
                candB={parent[b],b};
                candA={a,b};//try later one first
                vector<int> tmp=findRedundantDirectedConnectionI(edges,candA);
                if(tmp[0]==-1) return candA;
                else return candB;
            }
            parent[b]=a;
        }
        return findRedundantDirectedConnectionI(edges, {-1,-1});
    }
};

/*
directed graph -> tree
1,2,..,n
one addtional edge

edges=[{u,v}]

return to removed edge, return the occured last one in edges
================================
check wisdomPeak:
notice 
- root ind==0, outd=0~x
- other ind==1, outd=0~x

case 1: one node has two parent
    ind==2, then cut one edge to it to resolve, but can't just blindly cut any one
        e.g. check 0->1, 1->2, 2->1

case 2: circle -> all ind==1
    similar to question "Redundant Connection", remove the first one make it a circle by unionFind
*/
```