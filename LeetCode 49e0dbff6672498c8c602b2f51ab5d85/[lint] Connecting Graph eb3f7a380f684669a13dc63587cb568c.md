# [lint] Connecting Graph

#: 589
Difficult: Medium
Tags: Union Find
link: https://www.lintcode.com/problem/connecting-graph/
Priority: Medium
Created time: August 25, 2022 10:46 PM
Last edited time: October 22, 2023 3:12 PM
practicedTimes: 1
source: jiuzhang, 算高UnionFind&Trie

Given `n` nodes in a graph labeled from `1` to `n`. There is no edges in the graph at beginning.

You need to support the following method:

1. `connect(a, b)`, add an edge to connect node `a` and node b`.
2. `query(a, b)`, check if two nodes are connected

Example:
Example 1:

```
Input:
ConnectingGraph(5)
query(1, 2)
connect(1, 2)
query(1, 3)
connect(2, 4)
query(1, 4)
Output:
[false,false,true]

```

Example 2:

```
Input:
ConnectingGraph(6)
query(1, 2)
query(2, 3)
query(1, 3)
query(5, 6)
query(1, 4)

Output:
[false,false,false,false,false]

```

```cpp
class ConnectingGraph {
public:

    vector<int> father;
    /*
    * @param n: An integer
    */ConnectingGraph(int n) {
        // do intialization if necessary
        father.resize(n+1,0);
        for(int i=1;i<=n;i++){
            father[i]=i;//parent is itself
        }
    }

    /*
     * @param a: An integer
     * @param b: An integer
     * @return: nothing
     */
    void connect(int a, int b) {
        int rootA=find(a);
        int rootB=find(b);
        if(rootA==rootB) return;
        father[rootB]=rootA;
    }

    /*
     * @param a: An integer
     * @param b: An integer
     * @return: A boolean
     */
    bool query(int a, int b) {
				int rootA=find(a);
        int rootB=find(b);
        return rootA==rootB;
    }

    int find(int a){
				if(a==father[a]) return a;
        father[a]=find(father[a]);
        return father[a];
    }
};
```