# Parallel Courses

#: 1136
Difficult: Medium
Tags: Graph, TopSorting
link: https://leetcode.com/problems/parallel-courses/description/
Priority: Low
Created time: December 16, 2023 10:45 AM
Last edited time: December 16, 2023 10:45 AM
source: googleFreq

You are given an integer `n`, which indicates that there are `n` courses labeled from `1` to `n`. You are also given an array `relations` where `relations[i] = [prevCoursei, nextCoursei]`, representing a prerequisite relationship between course `prevCoursei` and course `nextCoursei`: course `prevCoursei` has to be taken before course `nextCoursei`.

In one semester, you can take **any number** of courses as long as you have taken all the prerequisites in the **previous** semester for the courses you are taking.

Return *the **minimum** number of semesters needed to take all courses*. If there is no way to take all the courses, return `-1`.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/02/24/course1graph.jpg](https://assets.leetcode.com/uploads/2021/02/24/course1graph.jpg)

```
Input: n = 3, relations = [[1,3],[2,3]]
Output: 2
Explanation: The figure above represents the given graph.
In the first semester, you can take courses 1 and 2.
In the second semester, you can take course 3.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/02/24/course2graph.jpg](https://assets.leetcode.com/uploads/2021/02/24/course2graph.jpg)

```
Input: n = 3, relations = [[1,2],[2,3],[3,1]]
Output: -1
Explanation: No course can be studied because they are prerequisites of each other.

```

**Constraints:**

- `1 <= n <= 5000`
- `1 <= relations.length <= 5000`
- `relations[i].length == 2`
- `1 <= prevCoursei, nextCoursei <= n`
- `prevCoursei != nextCoursei`
- All the pairs `[prevCoursei, nextCoursei]` are **unique**.

```cpp
class Solution {
public:
    int minimumSemesters(int n, vector<vector<int>>& relations) {
        //topsort
        //build graph
        //get ind
        vector<unordered_set<int>> g(n);//to use vector<vector>
        vector<int> ind(n,0);
        for(auto& e: relations){
            int u=e[0]-1, v=e[1]-1;
            g[u].insert(v);
            ind[v]++;
        }
        //queue to get seq
        queue<int> q;
        for(int i=0; i<n; i++){
            if(ind[i]==0) q.push(i);
        }

        int step=0;
        int cnt=0;
        while(!q.empty()){
            int qSz=q.size();
            for(int i=0; i<qSz; i++){
                int u=q.front(); q.pop();
                cnt++;
                for(auto v: g[u]){
                    ind[v]--;
                    if(ind[v]==0) q.push(v);
                }
            }
            step++;
        }
        if(cnt<n) return -1;
        return step;
    }
};

//T: O(V+E)
//S: O(V)
```