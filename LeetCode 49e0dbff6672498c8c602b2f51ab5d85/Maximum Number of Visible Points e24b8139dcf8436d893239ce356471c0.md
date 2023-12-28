# Maximum Number of Visible Points

#: 1610
Difficult: Hard
Tags: 2pt_SameDir_maxWin_chkValidAftR, Geometry, Math, Sliding Window, circle
link: https://leetcode.com/problems/maximum-number-of-visible-points/
Priority: Low
Created time: October 3, 2022 11:44 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, googleFreq

You are given an array `points`, an integer `angle`, and your `location`, where `location = [posx, posy]` and `points[i] = [xi, yi]` both denote **integral coordinates** on the X-Y plane.

Initially, you are facing directly east from your position. You **cannot move** from your position, but you can **rotate**. In other words, `posx` and `posy` cannot be changed. Your field of view in **degrees** is represented by `angle`, determining how wide you can see from any given view direction. Let `d` be the amount in degrees that you rotate counterclockwise. Then, your field of view is the **inclusive** range of angles `[d - angle/2, d + angle/2]`.

You can **see** some set of points if, for each point, the **angle** formed by the point, your position, and the immediate east direction from your position is **in your field of view**.

There can be multiple points at one coordinate. There may be points at your location, and you can always see these points regardless of your rotation. Points do not obstruct your vision to other points.

Return *the maximum number of points you can see*.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/09/30/89a07e9b-00ab-4967-976a-c723b2aa8656.png](https://assets.leetcode.com/uploads/2020/09/30/89a07e9b-00ab-4967-976a-c723b2aa8656.png)

```
Input: points = [[2,1],[2,2],[3,3]], angle = 90, location = [1,1]
Output: 3
Explanation: The shaded region represents your field of view. All points can be made visible in your field of view, including [3,3] even though [2,2] is in front and in the same line of sight.

```

**Example 2:**

```
Input: points = [[2,1],[2,2],[3,4],[1,1]], angle = 90, location = [1,1]
Output: 4
Explanation: All points can be made visible in your field of view, including the one at your location.

```

**Example 3:**

![https://assets.leetcode.com/uploads/2020/09/30/5010bfd3-86e6-465f-ac64-e9df941d2e49.png](https://assets.leetcode.com/uploads/2020/09/30/5010bfd3-86e6-465f-ac64-e9df941d2e49.png)

```
Input: points = [[1,0],[2,1]], angle = 13, location = [1,1]
Output: 1
Explanation: You can only see one of the two points, as shown above.

```

**Constraints:**

- `1 <= points.length <= 105`
- `points[i].length == 2`
- `location.length == 2`
- `0 <= angle < 360`
- `0 <= posx, posy, xi, yi <= 100`

```cpp
#define PI 3.14159265
#define MARGIN 1e-9

//sliding window
class Solution {
public:
    int visiblePoints(vector<vector<int>>& points, int inputAngle, vector<int>& location) {
        for(auto& p: points){
            p[0]-=location[0];
            p[1]-=location[1];
        }
        vector<double> angles;
        int base=0;
        for(auto& p: points){
            int x=p[0],y=p[1];
            if(x==0 && y==0) base++;
            else {
                double angle = atan2(y, x) * (180 / PI);
                angles.push_back(angle);
            }
        }
        sort(angles.begin(),angles.end());
        
        //duplicate array since it is a circle
        int cn=angles.size();
        for(int i=0; i<cn; i++){
            angles.push_back(angles[i]+360);
        }
        
        //sliding window
        int maxSeen=0;
        for(int l=0,r=0;r<angles.size();r++){
            while(angles[r]-angles[l]>inputAngle){//invalid
                l++;
            }
            maxSeen=max(maxSeen, r-l+1);
        }
        
        return maxSeen+base;
    }
};
```