# Kth Largest Element in an Array

#: 215
Difficult: Medium
Tags: PQ, QuickSelect, kth
link: https://leetcode.com/problems/kth-largest-element-in-an-array
Priority: Low
Created time: August 8, 2022 11:39 PM
Last edited time: October 19, 2023 10:06 PM
practicedTimes: 1
source: LC_75, LC_Top_Interview_150, jiuzhang, leetcode_daily, 算初TwoPointers, 算高FollowUp

Given an integer array `nums` and an integer `k`, return *the* `kth` *largest element in the array*.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

You must solve it in `O(n)` time complexity.

**Example 1:**

```
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5

```

**Example 2:**

```
Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
Output: 4

```

**Constraints:**

- `1 <= k <= nums.length <= 105`
- `104 <= nums[i] <= 104`

```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        return quickSelect(k, nums, 0, nums.size()-1);
    }
    
    int quickSelect(int k, vector<int>& nums, int start, int end){
        if(start==end){return nums[start];}
        
        int l=start, r=end;
        int mid=start+(end-start)/2;
        int pivot=nums[mid];
        while(l<=r){
            while(l<=r && nums[l]>pivot) l++;
            while(l<=r && nums[r]<pivot) r--;
            if(l<=r){
                swap(nums[l++],nums[r--]);
            }
        }
            if(k-1<=r){return quickSelect(k, nums, start, r);}
            else if(k-1>=l){return quickSelect(k, nums, l, end);}
            else{return nums[r+1];}
    }
};
```