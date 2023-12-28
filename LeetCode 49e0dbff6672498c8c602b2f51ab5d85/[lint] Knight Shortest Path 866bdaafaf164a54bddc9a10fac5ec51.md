# [lint] Knight Shortest Path

#: 611
Difficult: Medium
Tags: BFS, Matrix, mbfs_preChkVisited_wSz
link: https://www.lintcode.com/problem/knight-shortest-path
Priority: Low
Created time: July 26, 2022 5:37 PM
Last edited time: October 16, 2023 1:03 AM
practicedTimes: 1
source: jiuzhang, 算初BFS

Given a knight in a chessboard (a binary matrix with `0` as empty and `1` as barrier) with a `source` position, find the shortest path to a `destination` position, return the length of the route.
Return `-1` if destination cannot be reached.

Example:
**Example 1:**

```
Input:
[[0,0,0],
[0,0,0],
[0,0,0]]
source = [2, 0] destination = [2, 2]
Output: 2
Explanation:
[2,0]->[0,1]->[2,2]

```

**Example 2:**

```
Input:
[[0,1,0],
[0,0,1],
[0,0,0]]
source = [2, 0] destination = [2, 2]
Output:-1

```

Challenge

Related Questions:
knight-shortest-path-iii,reach-a-number,portal,knight-shortest-path-ii,search-graph-nodes

Tags:
NetEase,Breadth-first Search,DP

```cpp
/**
 * Definition for a point.
 * struct Point {
 *     int x, y;
 *     Point() : x(0), y(0) {}
 *     Point(int a, int b) : x(a), y(b) {}
 * };
 */

class Solution {
public:
	/**
	* @param grid a chessboard included 0 (false) and 1 (true)
	* @param source, destination a point
	* @return the shortest path
	*/
	int shortestPath(vector<vector<bool>>& grid, Point& source, Point& destination) {
		// Write your code here
		int m = grid.size();
		int n = grid[0].size();

		int depth = 0;
		vector<vector<bool>> visit(m, vector<bool>(n, false));//also used as visit set to dedup

		visit[source.x][source.y] = true;

		vector<vector<int>> d = { { -2, -1 }, { -2, 1 }, { -1, 2 }, { 1, 2 }, { 2, 1 }, { 2, -1 }, { 1, -2 }, { -1, -2 } };

		queue<Point> q;
		q.push(source);

		while (!q.empty()) {
			int sz = q.size();
			for (int i = 0; i < sz; i++){
				Point cur = q.front(); q.pop();
				if (cur.x == destination.x && cur.y == destination.y) return depth;

				for (int k = 0; k < 8; ++k) {//find all next points
					int x = cur.x + d[k][0];
					int y = cur.y + d[k][1];
					if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == 0 && !visit[x][y]) {
						visit[x][y] = true;
						q.push(Point(x, y));
					}
				}
			}
			depth++;
		}
		
		return -1;
	}
};
```