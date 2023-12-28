# Binary Search

#: 704
Difficult: Easy
Tags: BS_Idx
link: https://leetcode.com/problems/binary-search/
Priority: Pass
Created time: June 24, 2022 6:17 PM
Last edited time: October 13, 2023 11:26 PM
practicedTimes: 1
source: HuaHua, jiuzhang, 算初BS

Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

```
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4

```

**Example 2:**

```
Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1

```

**Constraints:**

- `1 <= nums.length <= 104`
- `104 < nums[i], target < 104`
- All the integers in `nums` are **unique**.
- `nums` is sorted in ascending order.

Solution 1:

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        auto itr=lower_bound(nums.begin(), nums.end(), target);
        if(itr!=nums.end() && *itr==target) return itr-nums.begin();
        return -1;
    }
};
```

Solution 2:

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n=nums.size();
        int l=0,r=n;
        while(l<r){
            int m=l+(r-l)/2;
            if(nums[m]<target) l=m+1;
            else r=m;
        }
        if(l!=n && nums[l]==target) return l;
        return -1;
    }
};
```

Solution old:

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n=nums.size();
        int l=0,r=n-1;
        while(l<r){
            int m=l+(r-l)/2;
            if(nums[m]<target) l=m+1;
            else r=m;
        }
        if(nums[l]==target) return l;
        return -1;
    }
}
```