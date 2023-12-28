# Maximum Score of a Node Sequence

#: 2242
Difficult: Hard
Tags: Graph, Greedy, OrderSet, PQ, Sort, TopK
link: https://leetcode.com/problems/maximum-score-of-a-node-sequence/
Priority: High
Status: No Idea At First, toSummarize
Created time: October 7, 2022 10:30 PM
Last edited time: October 12, 2023 2:56 PM
source: googleFreq

There is an **undirected** graph with `n` nodes, numbered from `0` to `n - 1`.

You are given a **0-indexed** integer array `scores` of length `n` where `scores[i]` denotes the score of node `i`. You are also given a 2D integer array `edges` where `edges[i] = [ai, bi]` denotes that there exists an **undirected** edge connecting nodes `ai` and `bi`.

A node sequence is **valid** if it meets the following conditions:

- There is an edge connecting every pair of **adjacent** nodes in the sequence.
- No node appears more than once in the sequence.

The score of a node sequence is defined as the **sum** of the scores of the nodes in the sequence.

Return *the **maximum score** of a valid node sequence with a length of* `4`*.* If no such sequence exists, return **`-1`.

**Example 1:**

![https://assets.leetcode.com/uploads/2022/04/15/ex1new3.png](https://assets.leetcode.com/uploads/2022/04/15/ex1new3.png)

```
Input: scores = [5,2,9,8,4], edges = [[0,1],[1,2],[2,3],[0,2],[1,3],[2,4]]
Output: 24
Explanation: The figure above shows the graph and the chosen node sequence [0,1,2,3].
The score of the node sequence is 5 + 2 + 9 + 8 = 24.
It can be shown that no other node sequence has a score of more than 24.
Note that the sequences [3,1,2,0] and [1,0,2,3] are also valid and have a score of 24.
The sequence [0,3,2,4] is not valid since no edge connects nodes 0 and 3.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2022/03/17/ex2.png](https://assets.leetcode.com/uploads/2022/03/17/ex2.png)

```
Input: scores = [9,20,6,4,11,12], edges = [[0,3],[5,3],[2,4],[1,3]]
Output: -1
Explanation: The figure above shows the graph.
There are no valid node sequences of length 4, so we return -1.

```

**Constraints:**

- `n == scores.length`
- `4 <= n <= 5 * 104`
- `1 <= scores[i] <= 108`
- `0 <= edges.length <= 5 * 104`
- `edges[i].length == 2`
- `0 <= ai, bi <= n - 1`
- `ai != bi`
- There are no duplicate edges.

top3

```cpp
/*
len of 4, 太牵强了条件给的
find mid edge in result candidate:

for(e: edge)
    a=e0, b=e1
    for each i: a->next
    for each j: b->next
    find largest sum from i->a->b->j

T(O) = nodes*edges*nodes

improve:
    make i the largest of a->next
    make j the largest of b->next
    
    issue: i might == b or j
    issue: j might == a or i
    
fix of improve: remember top 3 largests of node, then if dup found, use next large one

top3:
   
*/

class Solution {
public:
    int maximumScore(vector<int>& scores, vector<vector<int>>& edges) {
        int n=scores.size();
        
        //buildGraphWithTop3Nexts
        // pq or set
        // since push pop for pq and set are both logN, only set_top=logN < pq_top=1, but there is no top so it is fine
        vector<set<pair<int,int>>> nxt(n);//node - top3{score, nxt}, put score and nxt together so either to sort
        for(auto& e:edges){
            int a=e[0], b=e[1];
            nxt[a].insert({scores[b],b});
            nxt[b].insert({scores[a],a});
            if(nxt[a].size()>3) nxt[a].erase(nxt[a].begin());
            if(nxt[b].size()>3) nxt[b].erase(nxt[b].begin());
        }
        
        int res=-1;
        for(auto& e: edges){
            int a=e[0],b=e[1];
            for(auto [_,i]: nxt[a]){
                for(auto [_,j]: nxt[b]){
                    if(i!=j && i!=b && j!=a){
                        res=max(res, scores[i]+scores[a]+scores[b]+scores[j]);
                    }
                }
            }
        }
        return res;
    }
};
```

Failure TL

```cpp
//dfs 4 times for each node with back tracking
class Solution {
public:
    unordered_map<int, vector<int>> graph;
    
    int maximumScore(vector<int>& scores, vector<vector<int>>& edges) {
        int n=scores.size();
        buildGraph(edges);
        int res=-1;
        vector<int> visited(n,0);
        for(int i=0; i<n; i++){
            visited[i]=1;
            helper(scores,res,visited,i,1,0);
            visited[i]=0;
        }
        return res;
    }
    
    void helper(vector<int>& scores, int& res, vector<int>& visited, int node, int k, int nodeSum){
        nodeSum+=scores[node];
        if(k==4) {res=max(res,nodeSum); return;}
        
        for(auto nxt: graph[node]){
            if(visited[nxt]) continue;
            visited[nxt]=1;
            helper(scores, res, visited, nxt, k+1, nodeSum);
            visited[nxt]=0;
        }
        
    }
    
    void buildGraph(vector<vector<int>>& edges){
        for(auto& e:edges){
            int l=e[0], r=e[1];
            graph[l].push_back(r);
            graph[r].push_back(l);
        }
    }
};
```

Failure 1

```cpp
//fuck, node idx 7 8 9 1 is not considered
/*
建模- 画图
node direction, from small idx to large idx
for each cur node from 0...n-1
    for each nxt node linked from cur
        nxt(k,maxScore)= max(k, curVal+nxtVal)
            k=1,2,3,4, k=curk+1
        res=max(res, nxt(4))
*/
//DP,
//state: end with cur, with len k
//fuc: cur one get from previous one -> cur one calcu next ones
class Solution {
public:
    int maximumScore(vector<int>& scores, vector<vector<int>>& edges) {
        int n=scores.size();
        int res=-1;
        //buildGraphMap
        auto graph = buildGraphMap(edges);//nodeIdx - vector of next nodeIdxs
        
        vector<vector<int>> f(n,vector<int>(5,0));//node ix - cur result for sequence lenth of k
        //init
        for(int i=0; i<n; i++){
            f[i][1]=scores[i];
        }
        
        for(int cur=0; cur<n; cur++){
            for(int k=1;k<=3;k++){//for each sequence length, at most 3
                if(!f[cur][k]) continue;//ingore if no value
                for(auto nxt: graph[cur]){
                    f[nxt][k+1]=max(f[nxt][k+1],f[cur][k]+scores[nxt]);
                    if(k+1==4){res=max(res,f[nxt][k+1]);}
                }
            }
        }
        return res;
    }
    
    unordered_map<int, vector<int>> buildGraphMap(vector<vector<int>>& edges){
        unordered_map<int, vector<int>> mp;
        for(auto& e: edges){
            sort(e.begin(), e.end());
            int start=e[0], end=e[1];
            mp[start].push_back(end);
        }
        return mp;
    }
};
```