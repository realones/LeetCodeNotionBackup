# Finding the Number of Visible Mountains

#: 2345
Difficult: Medium
Tags: Array, Sort, monotonic, stack
link: https://leetcode.com/problems/finding-the-number-of-visible-mountains/
Priority: Medium
Status: HardToImplement, More Solution
Created time: December 16, 2023 2:54 AM
Last edited time: December 16, 2023 2:55 AM
source: googleFreq

You are given a **0-indexed** 2D integer array `peaks` where `peaks[i] = [xi, yi]` states that mountain `i` has a peak at coordinates `(xi, yi)`. A mountain can be described as a right-angled isosceles triangle, with its base along the `x`-axis and a right angle at its peak. More formally, the **gradients** of ascending and descending the mountain are `1` and `-1` respectively.

A mountain is considered **visible** if its peak does not lie within another mountain (including the border of other mountains).

Return *the number of visible mountains*.

**Example 1:**

![https://assets.leetcode.com/uploads/2022/07/19/ex1.png](https://assets.leetcode.com/uploads/2022/07/19/ex1.png)

```
Input: peaks = [[2,2],[6,3],[5,4]]
Output: 2
Explanation: The diagram above shows the mountains.
- Mountain 0 is visible since its peak does not lie within another mountain or its sides.
- Mountain 1 is not visible since its peak lies within the side of mountain 2.
- Mountain 2 is visible since its peak does not lie within another mountain or its sides.
There are 2 mountains that are visible.
```

**Example 2:**

![https://assets.leetcode.com/uploads/2022/07/19/ex2new1.png](https://assets.leetcode.com/uploads/2022/07/19/ex2new1.png)

```
Input: peaks = [[1,3],[1,3]]
Output: 0
Explanation: The diagram above shows the mountains (they completely overlap).
Both mountains are not visible since their peaks lie within each other.

```

**Constraints:**

- `1 <= peaks.length <= 105`
- `peaks[i].length == 2`
- `1 <= xi, yi <= 105`

```cpp
using ii=pair<int,int>;

class Solution {
public:
    int visibleMountains(vector<vector<int>>& peaks) {
        vector<ii> v;
        for(auto& p: peaks){
            int x=p[0], y=p[1];
            int x1=x-y, x2=x+y;
            v.emplace_back(x1,x2);
        }
        sort(v.begin(), v.end(), [&](ii a, ii b)->bool{
            auto [ax1,ax2] = a;
            auto [bx1,bx2] = b;
            if(ax1!=bx1) return ax1<bx1;
            else return ax2>bx2;
        });
        int res=0;
        int preX1=INT_MIN/2, preX2=INT_MIN/2;
        bool mountainRm=false;
        for(auto [x1,x2]: v){
            if(x2>preX2) {//new mountain can see
                res++;
                preX1=x1, preX2=x2;
                mountainRm=false;
            }
            else if(x1==preX1 && x2==preX2){
                if(mountainRm==false){
                    res--;  
                    mountainRm=true;
                }
            } 
        }
        return res;
    }
};
```