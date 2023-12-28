# [lint] Connecting Graph II

#: 590
Difficult: Medium
Tags: Union Find
link: https://www.lintcode.com/problem/connecting-graph-ii/
Priority: Pass
Created time: August 25, 2022 11:31 PM
Last edited time: October 22, 2023 7:07 PM
practicedTimes: 1
source: jiuzhang, 算高UnionFind&Trie

Given `n` nodes in a graph labeled from `1` to `n`. There is no edges in the graph at beginning.

You need to support the following method:

1. `connect(a, b)`, an edge to connect node a and node b
2. `query(a)`, Returns the number of connected component nodes which include node `a`.

Example:
Example 1:

```
Input:
ConnectingGraph2(5)
query(1)
connect(1, 2)
query(1)
connect(2, 4)
query(1)
connect(1, 4)
query(1)
Output:[1,2,3,3]

```

Example 2:

```
Input:
ConnectingGraph2(6)
query(1)
query(2)
query(1)
query(5)
query(1)

Output:
[1,1,1,1,1]

```

```cpp
class ConnectingGraph2 {
public:

    vector<int> father;
    vector<int> sz;
    /*
    * @param n: An integer
    */ConnectingGraph2(int n) {
        // do intialization if necessary
        father.resize(n+1,0);
        for(int i=1;i<n+1;i++){
            father[i]=i;//parent is itself
        }

        sz.resize(n+1,1);
    }

    /*
     * @param a: An integer
     * @param b: An integer
     * @return: nothing
     */
    void connect(int a, int b) {
        // write your code here
        int roota=find(a);
        int rootb=find(b);
        if(roota!=rootb) {
            father[rootb]=roota;
            sz[roota]+=sz[rootb];
        }
    }

    /*
     * @param a: An integer
     * @return: An integer
     */
    int query(int a) {
        // write your code here
        int roota=find(a);
        return sz[roota];
    }

    int find(int a){
				if(a==father[a]) return a;
        father[a]=find(father[a]);
        return father[a];
    }
};
```