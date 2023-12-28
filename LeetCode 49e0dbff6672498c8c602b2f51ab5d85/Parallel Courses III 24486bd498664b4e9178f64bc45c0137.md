# Parallel Courses III

#: 2050
Difficult: Hard
Tags: Array, DP, Graph, TopSorting
link: https://leetcode.com/problems/parallel-courses-iii/
Priority: Medium
Status: More Solution
Created time: October 17, 2023 6:13 PM
Last edited time: October 17, 2023 6:14 PM
source: leetcode_daily

You are given an integer `n`, which indicates that there are `n` courses labeled from `1` to `n`. You are also given a 2D integer array `relations` where `relations[j] = [prevCoursej, nextCoursej]` denotes that course `prevCoursej` has to be completed **before** course `nextCoursej` (prerequisite relationship). Furthermore, you are given a **0-indexed** integer array `time` where `time[i]` denotes how many **months** it takes to complete the `(i+1)th` course.

You must find the **minimum** number of months needed to complete all the courses following these rules:

- You may start taking a course at **any time** if the prerequisites are met.
- **Any number of courses** can be taken at the **same time**.

Return *the **minimum** number of months needed to complete all the courses*.

**Note:** The test cases are generated such that it is possible to complete every course (i.e., the graph is a directed acyclic graph).

**Example 1:**

![https://assets.leetcode.com/uploads/2021/10/07/ex1.png](https://assets.leetcode.com/uploads/2021/10/07/ex1.png)

```
Input: n = 3, relations = [[1,3],[2,3]], time = [3,2,5]
Output: 8
Explanation: The figure above represents the given graph and the time required to complete each course.
We start course 1 and course 2 simultaneously at month 0.
Course 1 takes 3 months and course 2 takes 2 months to complete respectively.
Thus, the earliest time we can start course 3 is at month 3, and the total time required is 3 + 5 = 8 months.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/10/07/ex2.png](https://assets.leetcode.com/uploads/2021/10/07/ex2.png)

```
Input: n = 5, relations = [[1,5],[2,5],[3,5],[3,4],[4,5]], time = [1,2,3,4,5]
Output: 12
Explanation: The figure above represents the given graph and the time required to complete each course.
You can start courses 1, 2, and 3 at month 0.
You can complete them after 1, 2, and 3 months respectively.
Course 4 can be taken only after course 3 is completed, i.e., after 3 months. It is completed after 3 + 4 = 7 months.
Course 5 can be taken only after courses 1, 2, 3, and 4 have been completed, i.e., after max(1,2,3,7) = 7 months.
Thus, the minimum time needed to complete all the courses is 7 + 5 = 12 months.

```

**Constraints:**

- `1 <= n <= 5 * 104`
- `0 <= relations.length <= min(n * (n - 1) / 2, 5 * 104)`
- `relations[j].length == 2`
- `1 <= prevCoursej, nextCoursej <= n`
- `prevCoursej != nextCoursej`
- All the pairs `[prevCoursej, nextCoursej]` are **unique**.
- `time.length == n`
- `1 <= time[i] <= 104`
- The given graph is a directed acyclic graph.

comment: i think it is just a simple topsort, why marked as a hard question, and with a dp tag?

```cpp
class Solution {
public:
    int minimumTime(int n, vector<vector<int>>& relations, vector<int>& time) {
        //change to 0-based
        for(auto& r: relations){ r[0]--, r[1]--; }
        vector<int> mp(n,0); //node_startTime
        //build graph
        unordered_map<int,int> ind;
        unordered_map<int,unordered_set<int>> g;
        for(auto& r:relations){
            int u=r[0], v=r[1];
            g[u].insert(v);
            ind[v]++;
        }
        //topsort
        queue<int> q;
        for(int i=0; i<n; i++){
            if(!ind[i]) q.push(i);
        }
        int res=0;
        while(!q.empty()){
            int u=q.front(); q.pop();
            int t=mp[u]+time[u];
            res=max(res,t);
            for(auto v: g[u]){
                ind[v]--;
                mp[v]=max(mp[v], t);
                if(ind[v]==0){
                    q.push(v);
                }
            }
        }
        return res;
    }
};

/*
n: 1~5e4

Return the minimum number of time needed to complete all the courses.
graph is always valid
================================
topSort with time
node: with start time
*/
```