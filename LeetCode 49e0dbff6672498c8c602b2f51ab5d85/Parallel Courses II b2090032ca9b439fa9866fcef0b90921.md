# Parallel Courses II

#: 1494
Difficult: Hard
Tags: Bit Manipulation, BitMask, DP, Graph
link: https://leetcode.com/problems/parallel-courses-ii/description/
Priority: High
Status: HardToImplement, No Idea At First, toSummarize
Created time: December 18, 2023 4:06 PM
Last edited time: December 18, 2023 7:45 PM
source: googleFreq

You are given an integer `n`, which indicates that there are `n` courses labeled from `1` to `n`. You are also given an array `relations` where `relations[i] = [prevCoursei, nextCoursei]`, representing a prerequisite relationship between course `prevCoursei` and course `nextCoursei`: course `prevCoursei` has to be taken before course `nextCoursei`. Also, you are given the integer `k`.

In one semester, you can take **at most** `k` courses as long as you have taken all the prerequisites in the **previous** semesters for the courses you are taking.

Return *the **minimum** number of semesters needed to take all courses*. The testcases will be generated such that it is possible to take every course.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/05/22/leetcode_parallel_courses_1.png](https://assets.leetcode.com/uploads/2020/05/22/leetcode_parallel_courses_1.png)

```
Input: n = 4, relations = [[2,1],[3,1],[1,4]], k = 2
Output: 3
Explanation: The figure above represents the given graph.
In the first semester, you can take courses 2 and 3.
In the second semester, you can take course 1.
In the third semester, you can take course 4.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/05/22/leetcode_parallel_courses_2.png](https://assets.leetcode.com/uploads/2020/05/22/leetcode_parallel_courses_2.png)

```
Input: n = 5, relations = [[2,1],[3,1],[4,1],[1,5]], k = 2
Output: 4
Explanation: The figure above represents the given graph.
In the first semester, you can only take courses 2 and 3 since you cannot take more than two per semester.
In the second semester, you can take course 4.
In the third semester, you can take course 1.
In the fourth semester, you can take course 5.

```

**Constraints:**

- `1 <= n <= 15`
- `1 <= k <= n`
- `0 <= relations.length <= n * (n-1) / 2`
- `relations[i].length == 2`
- `1 <= prevCoursei, nextCoursei <= n`
- `prevCoursei != nextCoursei`
- All the pairs `[prevCoursei, nextCoursei]` are **unique**.
- The given graph is a directed acyclic graph.

todo: prove the wrong solution is not correct

Solution wisdompeak:

```cpp
class Solution {
public:
    int minNumberOfSemesters(int n, vector<vector<int>>& relations, int k) {
        int M=(1<<n);
        vector<int> dp(M,INT_MAX/2);
        vector<int> preReq(M,M);
        unordered_map<int,unordered_set<int>> g;
        //build dependency graph
        for(auto& e: relations){
            int u=e[0]-1, v=e[1]-1;
            g[v].insert(u);
        }
        // calcu preReq[s]
        for(int s=0; s<M; s++){
            int req=0;
            for(int i=0;i<n;i++){//i is each node existing in s
                if((s>>i)&1){
                    for(auto u:g[i]){
                        req|=(1<<u);
                    }
                }
            }
            preReq[s]=req;
        }
        // dp[state]: min steps from 00000->state
        dp[0]=0;
        for(int s=1;s<M;s++){
            //1. prevState is a subset of state
            bool toBreak=false;
            for(int subset=s; subset>=0; subset=(subset-1)&s){
                if(subset==0) {
                    if(toBreak) break;
                    toBreak=true;
                }
                //2. countOne(state) - countOne(prevState) <=k
                if(__builtin_popcount(s) - __builtin_popcount(subset) > k) continue;
                //3. prevState must contains prerequisites of state
                if((subset & preReq[s]) != preReq[s]) continue;
                dp[s] = min(dp[s], dp[subset]+1);
            }
        }
        return dp.back();
    }
};

/*
NP: similar to TSP permutation

dp[state]: min steps from 00000->state

dp[state] min= dp[preState]+1
    prevState -> state
    1. prevState is a subset of state
    2. countOne(state) - countOne(prevState) <=k
    3. prevState must contains prerequisites of state

init:
    dp[00000]=0;

return:
    dp[2^n-1]
*/
```

Solution wrong - topSort w/ depth pq, greedy

failed case:

> n = 13
relations =
[[12,8],[2,4],[3,7],[6,8],[11,8],[9,4],[9,7],[12,4],[11,4],[6,4],[1,4],[10,7],[10,4],[1,7],[1,8],[2,7],[8,4],[10,8],[12,7],[5,4],[3,4],[11,7],[7,4],[13,4],[9,8],[13,8]]
k = 9
> 

```cpp
class Solution {
public:
    int minNumberOfSemesters(int n, vector<vector<int>>& relations, int k) {
        //build graph and indegree
        unordered_map<int,unordered_set<int>> g;
        unordered_map<int,int> ind;
        for(auto& e: relations){
            int u=e[0]-1, v=e[1]-1;
            g[u].insert(v);
            ind[v]++;
        }
        //build reverse graph and ind
        unordered_map<int,unordered_set<int>> reG;
        unordered_map<int,int> reInd;
        for(auto& e: relations){
            int v=e[0]-1, u=e[1]-1;
            reG[u].insert(v);
            reInd[v]++;
        }
        //calcu depth from core by reverse graph, topSort
        vector<int> depth(n,0);//node_depth
        queue<int> q;
        for(int i=0;i<n;i++){
            if(reInd[i]==0){
                q.push(i);
            }
        }
        while(!q.empty()){
            int u=q.front(); q.pop();
            for(auto v: reG[u]){
                reInd[v]--;
                if(reInd[v]==0){
                    q.push(v);
                    depth[v]=depth[u]+1;
                }
            }
        }
        // greedy: always handle the farthest nodes from core, using topSort on g and ind
        using ii=pair<int,int>;
        priority_queue<ii,vector<ii>,greater<>> pq;//[(depth, node)] when ind==0 sort by depth
        for(int i=0;i<n;i++){
            if(ind[i]==0){
                pq.emplace(depth[i],i);
            }
        }
        int step=0;
        while(!pq.empty()){
            int sz=pq.size();
            for(int i=0; i<sz && i<k; i++){//cur layer
                auto [_minD,u]=pq.top(); pq.pop();
                for(auto v: g[u]){
                    ind[v]--;
                    if(ind[v]==0){
                        pq.emplace(depth[v],v);
                    }
                }
            }
            step++;
        }
        return step;
    }
};

/*
solution topsort + greedy?
greedily process all node with ind==0, then next layer, ...?
    doesn't work for case, k=2:
        x->x->x->x->z
                a->
                b->
                c->
                d->
    best solution should be (a,x) then (b,x), (c,x) (d,x), (z)

solution greedy fetch depthest node:
    calcu depth of each node from the core
    always handle the farthest nodes from core

solution dp:???
dp[mask][bit]: min path distance from 00000->11111
    mask: bitmask of visited nodes, or name it state
    bit: last visited node

dp[mask][bit]=
    ###CUR###
    for each mask: 00000->11111
        for each valid bit [ind==0], backtracking to maintain ind
            ###PRE###
            for each preMask = mask-bit
                for each valid preBit [ind==0]
                    dp[mask][bit] min= dp[preMask][preBit] + dist(preBit,bit)

    dist(preBit,bit) is 

init:
    for each valid init node
        dp[node][node]=1;

return:
    dp[111111][any].min
*/
```