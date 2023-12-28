# [lint] Topological Sorting

#: 127
Difficult: Medium
Tags: TopSorting
link: https://www.lintcode.com/problem/127/
Priority: Low
Created time: July 17, 2022 11:52 PM
Last edited time: October 16, 2023 1:02 AM
practicedTimes: 1
source: jiuzhang, 算初BFS

**Description**

Given an directed graph, a topological order of the graph nodes is defined as follow:

- For each directed edge `A -> B` in graph, A must before B in the order list.
- The first node in the order can be any node in the graph with no nodes direct to it.

Find any topological order for the given graph.

- You can assume that there is at least one topological order in the graph.

[Learn more about representation of graphs](http://www.lintcode.com/help/graph)

- The number of graph nodes <= 5000

**Example**

**Example 1:**

Input:

```
graph = {0,1,2,3#1,4#2,4,5#3,4,5#4#5}

```

Output:

```
[0, 1, 2, 3, 4, 5]

```

Explanation:

For graph as follow:

![https://media-cdn.jiuzhang.com/markdown/images/8/6/91cf07d2-b7ea-11e9-bb77-0242ac110002.jpg](https://media-cdn.jiuzhang.com/markdown/images/8/6/91cf07d2-b7ea-11e9-bb77-0242ac110002.jpg)

he topological order can be:[0, 1, 2, 3, 4, 5][0, 2, 3, 1, 5, 4]...You only need to return any topological order for the given graph.

**Challenge**

Can you do it in both BFS and DFS?

```cpp
/**
 * Definition for Directed graph.
 * struct DirectedGraphNode {
 *     int label;
 *     vector<DirectedGraphNode *> neighbors;
 *     DirectedGraphNode(int x) : label(x) {};
 * };
 */

class Solution {
public:
    /**
     * @param graph: A list of Directed graph node
     * @return: Any topological order for the given graph.
     */
    vector<DirectedGraphNode*> topSort(vector<DirectedGraphNode*> graph) {
        // write your code here
        vector<DirectedGraphNode*> res;
        if(graph.size()==0) return res;
        //calculate in degree for each node
        unordered_map<DirectedGraphNode*, int> ind;
        for(auto node: graph){
            for(auto nei: node->neighbors){
                ind[nei]++;
            }
        }
        //push nodes with 0 indegree into queue
        queue<DirectedGraphNode*> q;
        for(auto node: graph){
            if(!ind.count(node)){
                q.push(node);
            }
        }
        //top sort and process - bfs
        while(!q.empty()){
            DirectedGraphNode* cur = q.front(); q.pop();
            //process
            res.push_back(cur);
            //recalculate indegree of neighbors of cur
            for(auto nei: cur->neighbors){
                ind[nei]--;
                if(ind[nei]==0){q.push(nei);}
            }
        }
        return res;
    }
};
```