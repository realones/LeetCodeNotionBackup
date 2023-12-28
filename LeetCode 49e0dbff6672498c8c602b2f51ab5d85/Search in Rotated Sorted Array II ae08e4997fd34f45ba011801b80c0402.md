# Search in Rotated Sorted Array II

#: 81
Difficult: Medium
Tags: BS_Idx, recursion
link: https://leetcode.com/problems/search-in-rotated-sorted-array-ii/description/
Priority: Medium
Created time: July 25, 2023 11:26 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Find Minimum in Rotated Sorted Array II (Find%20Minimum%20in%20Rotated%20Sorted%20Array%20II%207d3fc5585d744ed6b420a83fa07fb508.md)

There is an integer array `nums` sorted in non-decreasing order (not necessarily with **distinct** values).

Before being passed to your function, `nums` is **rotated** at an unknown pivot index `k` (`0 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,4,4,5,6,6,7]` might be rotated at pivot index `5` and become `[4,5,6,6,7,0,1,2,4,4]`.

Given the array `nums` **after** the rotation and an integer `target`, return `true` *if* `target` *is in* `nums`*, or* `false` *if it is not in* `nums`*.*

You must decrease the overall operation steps as much as possible.

**Example 1:**

```
Input: nums = [2,5,6,0,0,1,2], target = 0
Output: true

```

**Example 2:**

```
Input: nums = [2,5,6,0,0,1,2], target = 3
Output: false

```

**Constraints:**

- `1 <= nums.length <= 5000`
- `104 <= nums[i] <= 104`
- `nums` is guaranteed to be rotated at some pivot.
- `104 <= target <= 104`

**Follow up:** This problem is similar to [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/description/), but `nums` may contain **duplicates**. Would this affect the runtime complexity? How and why?

Solution:

since need to consider both side for nums[m]==num[r] situation, use recursive instead of iterative

```cpp
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        return search(nums,target,0,nums.size()-1);
    }
    bool search(vector<int>& nums, int target, int l, int r) {
        if(l>=r) return nums[l]==target;

        int m=l+(r-l)/2;
        if(nums[m]<=nums[r]){
            int nl=l,nr=r;
            if(nums[m]<target && target<=nums[r]) nl=m+1;
            else nr=m; 
            if(search(nums,target,nl,nr)) return true;
        }
        if(nums[m]>=nums[r]){
            int nl=l,nr=r;
            if(nums[m]<target || target<=nums[r]) nl=m+1;
            else nr=m; 
            if(search(nums,target,nl,nr)) return true;
        }
        return false;
    }
};
```