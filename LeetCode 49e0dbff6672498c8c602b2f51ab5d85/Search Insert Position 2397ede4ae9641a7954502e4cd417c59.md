# Search Insert Position

#: 35
Difficult: Easy
Tags: BS_Idx
link: https://leetcode.com/problems/search-insert-position/
Priority: Pass
Created time: June 6, 2023 10:56 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_Top_Interview_150

Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

```
Input: nums = [1,3,5,6], target = 5
Output: 2

```

**Example 2:**

```
Input: nums = [1,3,5,6], target = 2
Output: 1

```

**Example 3:**

```
Input: nums = [1,3,5,6], target = 7
Output: 4

```

**Constraints:**

- `1 <= nums.length <= 104`
- `104 <= nums[i] <= 104`
- `nums` contains **distinct** values sorted in **ascending** order.
- `104 <= target <= 104`

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int n=nums.size(); if(n==0) {return 0;}
        int l=0,r=n-1;
        while(l<r){
            int m=l+(r-l)/2;
            if(nums[m]<target) {l=m+1;}
            else {r=m;}
        }
        if(nums[l]>=target) {return l;}
        return n;
    }
};
```