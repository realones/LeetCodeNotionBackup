# Find First and Last Position of Element in Sorted Array

#: 34
Difficult: Medium
Tags: BS_Idx
link: https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/
Priority: Pass
Created time: June 7, 2023 12:24 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_Top_Interview_150

Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]

```

**Example 2:**

```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]

```

**Example 3:**

```
Input: nums = [], target = 0
Output: [-1,-1]

```

**Constraints:**

- `0 <= nums.length <= 105`
- `109 <= nums[i] <= 109`
- `nums` is a non-decreasing array.
- `109 <= target <= 109`

```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        vector<int> res{-1,-1};
        int n=nums.size(); if(n==0) return res;
        int left=-1,right=-1;
        int l=0,r=n-1;
        while(l<=r){
            int m=l+(r-l)/2;
            if(nums[m]<target) l=m+1;
            else r=m-1;
        }
        if(l==n || nums[l]!=target) return res;
        left=l;
        
        l=0,r=n-1;
        while(l<=r){
            int m=l+(r-l)/2;
            if(nums[m]>target) r=m-1;
            else l=m+1;
        }
        if(r==-1 || nums[r]!=target) return res;
        right=r;
        
        return vector<int>{left, right};
    }
};
```