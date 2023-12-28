# Check If It Is a Straight Line

#: 1232
Difficult: Easy
Tags: Geometry, Math
link: https://leetcode.com/problems/check-if-it-is-a-straight-line/
Priority: Low
Created time: June 4, 2023 9:17 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

You are given an array `coordinates`, `coordinates[i] = [x, y]`, where `[x, y]` represents the coordinate of a point. Check if these points make a straight line in the XY plane.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/10/15/untitled-diagram-2.jpg](https://assets.leetcode.com/uploads/2019/10/15/untitled-diagram-2.jpg)

```
Input: coordinates = [[1,2],[2,3],[3,4],[4,5],[5,6],[6,7]]
Output: true

```

**Example 2:**

![https://assets.leetcode.com/uploads/2019/10/09/untitled-diagram-1.jpg](https://assets.leetcode.com/uploads/2019/10/09/untitled-diagram-1.jpg)

```
Input: coordinates = [[1,1],[2,2],[3,4],[4,5],[5,6],[7,7]]
Output: false

```

**Constraints:**

- `2 <= coordinates.length <= 1000`
- `coordinates[i].length == 2`
- `10^4 <= coordinates[i][0], coordinates[i][1] <= 10^4`
- `coordinates` contains no duplicate point.

```cpp
class Solution {
public:
    bool checkStraightLine(vector<vector<int>>& coordinates) {
        int n=coordinates.size();
        int x0=coordinates[0][0], y0=coordinates[0][1];
        int x1=coordinates[1][0], y1=coordinates[1][1];
        
        if(x0==x1){
            for(int i=2;i<n;i++){
                if(coordinates[i][0]!=x0) return false;
            }
        }
        else if(y0==y1){
            for(int i=2;i<n;i++){
                if(coordinates[i][1]!=y0) return false;
            }
        }
        else{
            for(int i=2;i<n;i++){
                int x=coordinates[i][0];
                int y=coordinates[i][1];
                if((y1-y0)*(x-x0)!=(x1-x0)*(y-y0)) return false;
            }
        }
        return true;
    }
};
```