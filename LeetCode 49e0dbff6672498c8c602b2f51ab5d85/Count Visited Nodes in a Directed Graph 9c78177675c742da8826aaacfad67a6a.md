# Count Visited Nodes in a Directed Graph

#: 2876
Difficult: Hard
Tags: DFS, DP, Graph, Hash, TopSorting, stack
link: https://leetcode.com/problems/count-visited-nodes-in-a-directed-graph/
Priority: High
Status: HardToImplement, More Solution, No Idea At First, toRevisit
Created time: October 5, 2023 1:34 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

There is a **directed** graph consisting of `n` nodes numbered from `0` to `n - 1` and `n` directed edges.

You are given a **0-indexed** array `edges` where `edges[i]` indicates that there is an edge from node `i` to node `edges[i]`.

Consider the following process on the graph:

- You start from a node `x` and keep visiting other nodes through edges until you reach a node that you have already visited before on this **same** process.

Return *an array* `answer` *where* `answer[i]` *is the number of **different** nodes that you will visit if you perform the process starting from node* `i`.

**Example 1:**

![https://assets.leetcode.com/uploads/2023/08/31/graaphdrawio-1.png](https://assets.leetcode.com/uploads/2023/08/31/graaphdrawio-1.png)

```
Input: edges = [1,2,0,0]
Output: [3,3,3,4]
Explanation: We perform the process starting from each node in the following way:
- Starting from node 0, we visit the nodes 0 -> 1 -> 2 -> 0. The number of different nodes we visit is 3.
- Starting from node 1, we visit the nodes 1 -> 2 -> 0 -> 1. The number of different nodes we visit is 3.
- Starting from node 2, we visit the nodes 2 -> 0 -> 1 -> 2. The number of different nodes we visit is 3.
- Starting from node 3, we visit the nodes 3 -> 0 -> 1 -> 2 -> 0. The number of different nodes we visit is 4.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2023/08/31/graaph2drawio.png](https://assets.leetcode.com/uploads/2023/08/31/graaph2drawio.png)

```
Input: edges = [1,2,3,4,0]
Output: [5,5,5,5,5]
Explanation: Starting from any node we can visit every node in the graph in the process.

```

**Constraints:**

- `n == edges.length`
- `2 <= n <= 105`
- `0 <= edges[i] <= n - 1`
- `edges[i] != i`

comment: 有思路但是很难清晰的捋通顺，实现起来磕磕绊绊，可能类似题目做得少。知道具体实现，但是写起来磕磕绊绊。

lee215有个只用stack的solution

Solution topsort:

```cpp
class Solution {
public:
    vector<int> countVisitedNodes(vector<int>& edges) {
        int n=edges.size();
        vector<int> res(n,0);

        //step 1: topSort to find non cycle nodes
        vector<int> ind(n,0); //course - ind
        for(int i=0; i<n; i++) ind[edges[i]]++;
        
        queue<int> q;
        for(int i=0; i<n; i++) if(!ind[i]) q.push(i);
        
        unordered_set<int> visited;
        vector<int> seq;
        while(!q.empty()){
            int u=q.front(); q.pop();
            visited.insert(u); seq.push_back(u);
            int v=edges[u];
            ind[v]--;
            if(ind[v]==0) q.push(v);
        }
        //step 2: calcu cycle sz and nodes
        for(int i=0; i<n; i++){ //for each nodes in cycle
            if(visited.count(i)) continue;
            vector<int> circleElems;
            int circleSz=0;
            for(int u=i; !visited.count(u); u=edges[u]){
                visited.insert(u);
                circleElems.push_back(u);
                circleSz++;
            }
            for(auto e: circleElems){ res[e]=circleSz; }
        }
        //step 3: reverse topSort order to calcu non-circle elems
        reverse(seq.begin(), seq.end());
        for(auto u: seq){
            res[u]=res[edges[u]]+1;
        }
        return res;
    }
};

/*
each node, ind=0~x, outd=1
can be more than one component
each node is in a circle, or in the way to a circle

step 1: find circles, 
    1. find the cycleSize: node cnt of a circle
    2. find the nodes in circle (which has res=cycleSize)
step 2: start from circle, find other nodes linked from the circle

================================
approach:
step1: topSort to find cycles
    calcu ind[node]
    for node ind==0, inq
    topsort bfs with dec nei's ind
visited ones: not in circle
non-visited ones: in circle

step2: find circle nodes and sz
for not visited nodes
    calcu cycle sz and mark as visited, so not traverse same circle again

step3: calcu nodes not in cycle
to first calculate the node that was directly connected to cycle:
reverse order of visited nodes in topSort
*/
```