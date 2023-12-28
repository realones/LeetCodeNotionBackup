# Jump Game VIII

#: 2297
Difficult: Medium
Tags: Array, DP, Dijkstra, Graph, monotonic, shortest_path, stack
link: https://leetcode.com/problems/jump-game-viii/
Priority: High
Status: HardToImplement, checkBetterSolution, toRevisit
Created time: November 14, 2023 1:43 AM
Last edited time: November 17, 2023 1:26 AM
practicedTimes: 1
source: AmazonFreq

You are given a **0-indexed** integer array `nums` of length `n`. You are initially standing at index `0`. You can jump from index `i` to index `j` where `i < j` if:

- `nums[i] <= nums[j]` and `nums[k] < nums[i]` for all indexes `k` in the range `i < k < j`, or
- `nums[i] > nums[j]` and `nums[k] >= nums[i]` for all indexes `k` in the range `i < k < j`.

You are also given an integer array `costs` of length `n` where `costs[i]` denotes the cost of jumping **to** index `i`.

Return *the **minimum** cost to jump to the index* `n - 1`.

**Example 1:**

```
Input: nums = [3,2,4,4,1], costs = [3,7,6,4,2]
Output: 8
Explanation: You start at index 0.
- Jump to index 2 with a cost of costs[2] = 6.
- Jump to index 4 with a cost of costs[4] = 2.
The total cost is 8. It can be proven that 8 is the minimum cost needed.
Two other possible paths are from index 0 -> 1 -> 4 and index 0 -> 2 -> 3 -> 4.
These have a total cost of 9 and 12, respectively.

```

**Example 2:**

```
Input: nums = [0,1,2], costs = [1,1,1]
Output: 2
Explanation: Start at index 0.
- Jump to index 1 with a cost of costs[1] = 1.
- Jump to index 2 with a cost of costs[2] = 1.
The total cost is 2. Note that you cannot jump directly from index 0 to index 2 because nums[0] <= nums[1].

```

**Constraints:**

- `n == nums.length == costs.length`
- `1 <= n <= 105`
- `0 <= nums[i], costs[i] <= 105`

comment: only beat 5%, check better solution, also, seems not need to use dijkstra, but use dp

```cpp
using ll=long long;
using lli=pair<ll,int>;

class Solution {
public:
    long long minCost(vector<int>& nums, vector<int>& costs) {
        int n=nums.size();//1~1e5
        stack<int> stk;
        unordered_map<int,unordered_set<int>> g;
        //build graph for rule1
        stk={};
        for(int i=0; i<n; i++){
            while(!stk.empty() && nums[stk.top()]<=nums[i]) {
                int tmp=stk.top(); stk.pop();
                g[tmp].insert(i);
            }
            stk.push(i);
        }
        //build graph for rule2
        stk={};
        for(int i=0; i<n; i++){
            while(!stk.empty() && nums[stk.top()]>nums[i]) {
                int tmp=stk.top(); stk.pop();
                g[tmp].insert(i);
            }
            stk.push(i);
        }
        //find shortest path from 0 to n-1, dijkstra
        //not sure if recursive from node(n-1) will work or not, seems also work since at most O(n) complexity?
        priority_queue<lli,vector<lli>,greater<>> pq; //[d,u]
        vector<ll> dist(n,LLONG_MAX);

        pq.push({0,0});//init node
        while(!pq.empty()){
            auto [d,u] = pq.top(); pq.pop();
            if(dist[u]!=LLONG_MAX) continue;
            dist[u]=d;

            for(auto v: g[u]){
                ll w=costs[v];
                if(dist[v]!=LLONG_MAX) continue;
                pq.push({d+w,v});
            }
        }

        return dist[n-1];
    }
};

/*
init: idx 0

i s s s j,  [i]<=[j]
 or
i le le le j,  [i]>[j]

return min cost
================================

dp[i]: min cost jump to idx i

dp[i] = 

init dp[0]=0

return dp[n-1]
================================
build graph?

init 0->1->2->...->n-1

mono stk?

     |
|    |
||  ||
|| |||
||||||

dec stk

or

*/
```