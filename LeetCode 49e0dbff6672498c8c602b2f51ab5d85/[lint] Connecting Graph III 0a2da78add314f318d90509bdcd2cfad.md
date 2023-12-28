# [lint] Connecting Graph III

#: 591
Difficult: Medium
Tags: Union Find
link: https://www.lintcode.com/problem/591/
Priority: Pass
Created time: August 25, 2022 11:50 PM
Last edited time: October 22, 2023 7:08 PM
practicedTimes: 1
source: jiuzhang, 算高UnionFind&Trie

Description

Given `n` nodes in a graph labeled from `1` to `n`. There is no edges in the graph at beginning.

You need to support the following method:

1. `connect(a, b)`, an edge to connect node a and node b
2. `query()`, Returns the number of connected component in the graph

*Contact me on wechat to get **Amazon、Google** requent Interview questions . (wechat id : **jiuzhang0607**)*

Example

Example 1:

```
Input:
ConnectingGraph3(5)
query()
connect(1, 2)
query()
connect(2, 4)
query()
connect(1, 4)
query()

Output:[5,4,3,3]

```

Example 2:

```
Input:
ConnectingGraph3(6)
query()
query()
query()
query()
query()

Output:
[6,6,6,6,6]

```

```cpp
class ConnectingGraph3 {
public:

    vector<int> father;
    int componentCnt;
    /**
     * @param a: An integer
     * @param b: An integer
     * @return: nothing
     */
    ConnectingGraph3(int n) {
        // initialize your data structure here.
        father.resize(n+1);
        for(int i=1; i<=n; i++){
            father[i]=i;
        }
        componentCnt=n;
    }
    void connect(int a, int b) {
        // write your code here
        int rootA=find(a);
        int rootB=find(b);
        if(rootA==rootB) return;
        father[rootB]=rootA;
        componentCnt--;
    }

    /**
     * @return: An integer
     */
    int query() {
        // write your code here
        return componentCnt;
    }

    int find(int a){
        if(a==father[a]) return a;
        father[a]=find(father[a]);
        return father[a];
    }
};
```