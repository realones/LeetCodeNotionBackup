# Find Minimum in Rotated Sorted Array II

#: 154
Difficult: Hard
Tags: BS_Idx, Divide and Conquer
link: https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/
Priority: Medium
Created time: June 21, 2023 8:22 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Search in Rotated Sorted Array II (Search%20in%20Rotated%20Sorted%20Array%20II%20ae08e4997fd34f45ba011801b80c0402.md), Find Minimum in Rotated Sorted Array (Find%20Minimum%20in%20Rotated%20Sorted%20Array%20b7069c3a155d4e638a6a3dc2f45319e7.md)

Suppose an array of length `n` sorted in ascending order is **rotated** between `1` and `n` times. For example, the array `nums = [0,1,4,4,5,6,7]` might become:

- `[4,5,6,7,0,1,4]` if it was rotated `4` times.
- `[0,1,4,4,5,6,7]` if it was rotated `7` times.

Notice that **rotating** an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array `nums` that may contain **duplicates**, return *the minimum element of this array*.

You must decrease the overall operation steps as much as possible.

**Example 1:**

```
Input: nums = [1,3,5]
Output: 1

```

**Example 2:**

```
Input: nums = [2,2,2,0,1]
Output: 0

```

**Constraints:**

- `n == nums.length`
- `1 <= n <= 5000`
- `5000 <= nums[i] <= 5000`
- `nums` is sorted and rotated between `1` and `n` times.

**Follow up:** This problem is similar to [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/), but `nums` may contain **duplicates**. Would this affect the runtime complexity? How and why?

Solution: divide and conquer to write BS

```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {
        if(nums.empty()) return 0;
        return findMin(nums,0,nums.size()-1);
    }
    int findMin(vector<int>& nums, int start, int end) {
        if(start>end) return INT_MAX;
        if(start==end) return nums[start];
        int mid=start+(end-start)/2;
        if(nums[mid]>nums[end]) return findMin(nums,mid+1,end);
        else if(nums[mid]<nums[end]) return findMin(nums,start,mid);
        return min(findMin(nums,mid+1,end), findMin(nums,start,mid));
    }
}
```