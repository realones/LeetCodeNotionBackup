# Find the Highest Altitude

#: 1732
Difficult: Easy
Tags: Array, Prefix
link: https://leetcode.com/problems/find-the-highest-altitude
Priority: Pass
Created time: June 18, 2023 5:05 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_75, leetcode_daily

There is a biker going on a road trip. The road trip consists of `n + 1` points at different altitudes. The biker starts his trip on point `0` with altitude equal `0`.

You are given an integer array `gain` of length `n` where `gain[i]` is the **net gain in altitude** between points `i` and `i + 1` for all (`0 <= i < n)`. Return *the **highest altitude** of a point.*

**Example 1:**

```
Input: gain = [-5,1,5,0,-7]
Output: 1
Explanation: The altitudes are [0,-5,-4,1,1,-6]. The highest is 1.

```

**Example 2:**

```
Input: gain = [-4,-3,-2,-1,4,3,2]
Output: 0
Explanation: The altitudes are [0,-4,-7,-9,-10,-6,-3,-1]. The highest is 0.

```

**Constraints:**

- `n == gain.length`
- `1 <= n <= 100`
- `100 <= gain[i] <= 100`

```cpp
class Solution {
public:
    int largestAltitude(vector<int>& gain) {
        int h=0,res=0;
        int n=gain.size();
        for(auto g: gain){
            h+=g;
            res=max(res,h);
        }
        return res;
    }
};
```