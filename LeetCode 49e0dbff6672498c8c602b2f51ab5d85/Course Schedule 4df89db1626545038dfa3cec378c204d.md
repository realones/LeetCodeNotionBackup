# Course Schedule

#: 207
Difficult: Medium
Tags: Graph, TopSorting
link: https://leetcode.com/problems/course-schedule/
Priority: Medium
Created time: July 25, 2022 3:40 PM
Last edited time: October 16, 2023 1:00 AM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, jiuzhang, 算初BFS

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

**Example 1:**

```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take.
To take course 1 you should have finished course 0. So it is possible.

```

**Example 2:**

```
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take.
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.

```

**Constraints:**

- `1 <= numCourses <= 2000`
- `0 <= prerequisites.length <= 5000`
- `prerequisites[i].length == 2`
- `0 <= ai, bi < numCourses`
- All the pairs prerequisites[i] are **unique**.

```cpp
class Solution {
public:
    /*
	* @param numCourses: a total of n courses
	* @param prerequisites: a list of prerequisite pairs
	* @return: true if can finish all courses or false
	*/
	bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
		// Write your code here
		vector<int> d(numCourses, 0);//in degree, we can use vector since course id from 0~n-1, otherwise use map
		vector<unordered_multiset<int>> graph(numCourses);//graph, same as above, no need to use map<int, set<int>>

		for (auto p : prerequisites) {//second is parent of first
			graph[p[1]].insert(p[0]);//build graph
			d[p[0]] ++;//build in degree
		}

		queue<int> q;
		for (int i = 0; i < numCourses; ++i)
			if (d[i] == 0) q.push(i);

		int canTake = 0;
		while (!q.empty()) {
			int course = q.front(); q.pop();
			canTake++;
			for (auto child : graph[course]) {
				d[child]--;
				if (d[child] == 0) {
					q.push(child);
				}
			}
		}
		return canTake == numCourses;
	}
};
```