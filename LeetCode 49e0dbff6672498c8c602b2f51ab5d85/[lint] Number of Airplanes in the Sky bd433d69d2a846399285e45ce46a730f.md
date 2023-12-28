# [lint] Number of Airplanes in the Sky

#: 391
Difficult: Medium
Tags: Hash, Sort, Sweep-Line
link: https://www.lintcode.com/problem/391/
Priority: Pass
Created time: September 9, 2022 10:49 AM
Last edited time: October 28, 2023 5:19 PM
practicedTimes: 1
source: HuaHua, jiuzhang, 算高BS&SwipeLine

Description

Given an list `interval`, which are taking off and landing time of the flight. How many airplanes are there at most at the same time in the sky?

# 

If landing and taking off of different planes happen at the same time, we consider landing should happen at first.

Example

**Example 1:**

```
Input: [(1, 10), (2, 3), (5, 8), (4, 7)]
Output: 3
Explanation:
The first airplane takes off at 1 and lands at 10.
The second ariplane takes off at 2 and lands at 3.
The third ariplane takes off at 5 and lands at 8.
The forth ariplane takes off at 4 and lands at 7.
During 5 to 6, there are three airplanes in the sky.

```

**Example 2:**

```
Input: [(1, 2), (2, 3), (3, 4)]
Output: 1
Explanation: Landing happen before taking off.

```

详细分析：[https://briangordon.github.io/2014/08/the-skyline-problem.html](https://briangordon.github.io/2014/08/the-skyline-problem.html) 没看

```cpp
/**
 * Definition of Interval:
 * classs Interval {
 *     int start, end;
 *     Interval(int start, int end) {
 *         this->start = start;
 *         this->end = end;
 *     }
 */

class Solution {
public:
    /*
     * @param airplanes: An interval array
     * @return: Count of airplanes are in the sky.
     */
    int countOfAirplanes(vector<Interval> &airplanes) {
        //element: {time,isStart}, use vector but not custom class, since we can directly sort vectors
        vector<vector<int>> points;
        for(auto plane:airplanes){
            points.push_back({plane.start,1});
            points.push_back({plane.end,0});
        }
        
        sort(points.begin(),points.end());
        int res=0;
        int cnt=0;
        for(auto p: points){
            if(p[1]) cnt++;
            else cnt--;
            res=max(res,cnt);
        }
        return res;
    }
};
```