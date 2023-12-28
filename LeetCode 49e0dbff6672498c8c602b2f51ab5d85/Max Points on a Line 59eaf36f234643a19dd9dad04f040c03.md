# Max Points on a Line

#: 149
Difficult: Hard
Tags: Geometry, Hash, Math
link: https://leetcode.com/problems/max-points-on-a-line/
Priority: High
Status: No Idea At First
Created time: June 7, 2023 1:37 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_Top_Interview_150

Given an array of `points` where `points[i] = [xi, yi]` represents a point on the **X-Y** plane, return *the maximum number of points that lie on the same straight line*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/02/25/plane1.jpg](https://assets.leetcode.com/uploads/2021/02/25/plane1.jpg)

```
Input: points = [[1,1],[2,2],[3,3]]
Output: 3

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/02/25/plane2.jpg](https://assets.leetcode.com/uploads/2021/02/25/plane2.jpg)

```
Input: points = [[1,1],[3,2],[5,3],[4,1],[2,3],[1,4]]
Output: 4

```

**Constraints:**

- `1 <= points.length <= 300`
- `points[i].length == 2`
- `104 <= xi, yi <= 104`
- All the `points` are **unique**.

```cpp
/**
 * Definition for a point.
 * struct Point {
 *     int x;
 *     int y;
 *     Point() : x(0), y(0) {}
 *     Point(int a, int b) : x(a), y(b) {}
 * };
 */
class Solution {
public:
	int maxPoints(vector<Point>& points) {
		int res = 0;
		//for each point, find line with all points
		//including it self, so not need to skip itself and test situation when points.size==1
		for (auto orig : points) {
			int same = 0;
            int tmpRes=0;
			unordered_map<string, int> mp;
			for (auto p : points) {
				int x = orig.x - p.x;
				int y = orig.y - p.y;
				if (x == 0 && y == 0) { same++; continue; }
				//not need to test vertical line and horizontal line
				//since gcd ganrantee x,y to be 1,0 or 0,1
				int gcd = getGcd(x, y);
				x /= gcd;
				y /= gcd;
				string slope = to_string(x) + " " + to_string(y);
				mp[slope]++;
				tmpRes = max(tmpRes, mp[slope]);
			}
            res=max(res,tmpRes+same);
		}
		return res;
	}
	//not need to care situation of negtive number
	//since gcd(1,-1)=1, gcd(-1,1)=-1, which make slope always correct
	int getGcd(int a, int b){
		if (b==0) return a;
		return getGcd(b,a%b);
	}
};
```