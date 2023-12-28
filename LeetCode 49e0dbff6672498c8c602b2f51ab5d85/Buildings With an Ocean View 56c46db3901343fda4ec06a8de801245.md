# Buildings With an Ocean View

#: 1762
Difficult: Medium
Tags: Array, monotonic, stack
link: https://leetcode.com/problems/buildings-with-an-ocean-view/description/
Priority: Low
Status: More Solution
Created time: November 23, 2023 3:22 AM
Last edited time: November 23, 2023 3:22 AM
source: facebookFreq

There are `n` buildings in a line. You are given an integer array `heights` of size `n` that represents the heights of the buildings in the line.

The ocean is to the right of the buildings. A building has an ocean view if the building can see the ocean without obstructions. Formally, a building has an ocean view if all the buildings to its right have a **smaller** height.

Return a list of indices **(0-indexed)** of buildings that have an ocean view, sorted in increasing order.

**Example 1:**

```
Input: heights = [4,2,3,1]
Output: [0,2,3]
Explanation: Building 1 (0-indexed) does not have an ocean view because building 2 is taller.

```

**Example 2:**

```
Input: heights = [4,3,2,1]
Output: [0,1,2,3]
Explanation: All the buildings have an ocean view.

```

**Example 3:**

```
Input: heights = [1,3,2,4]
Output: [3]
Explanation: Only building 3 has an ocean view.

```

**Constraints:**

- `1 <= heights.length <= 105`
- `1 <= heights[i] <= 109`

```cpp
class Solution {
public:
    vector<int> findBuildings(vector<int>& nums) {
        int n=nums.size();// if(n==0) {return 0;}
        int curMax=INT_MIN;
        vector<int> res;
        for(int i=n-1;i>=0;i--){
            if(nums[i]>curMax) res.push_back(i);
            curMax=max(curMax, nums[i]);
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```