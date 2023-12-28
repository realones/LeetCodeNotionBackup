# [lint] Recover Rotated Sorted Array

#: 39
Difficult: Easy
Tags: 3 Step Rotate
link: https://www.lintcode.com/problem/recover-rotated-sorted-array
Priority: Low
Created time: July 8, 2022 4:26 PM
Last edited time: October 19, 2023 9:33 PM
practicedTimes: 1
source: jiuzhang, 算初BS, 算初TwoPointers

Given a **rotated** sorted array, recover it to sorted array in-place.（Ascending）

Example:
**Example1:**`[4, 5, 1, 2, 3]` -> `[1, 2, 3, 4, 5]`**Example2:**`[6,8,9,1,2]` -> `[1,2,6,8,9]`

Challenge
In-place, O(*1*) extra space and O(*n*) time.

Related Questions:
rotate-string-ii,sort-colors,rotate-string

no bs required as already O(n)

```cpp
class Solution {
public:
    /*
     * @param nums: An integer array
     * @return: nothing
     */
    void recoverRotatedSortedArray(vector<int> &nums) {
        int n=nums.size(); if(n==0) return;
        int sz1=0;
        for(int i=1;i<n;i++){
            if(nums[i-1]>nums[i]){sz1=i;break;}
        }
        if(sz1==0) return;//already sorted

        reverse(nums.begin(),nums.begin()+sz1);
        reverse(nums.begin()+sz1,nums.end());
        reverse(nums.begin(),nums.end());
    }
};
```