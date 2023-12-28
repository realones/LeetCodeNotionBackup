# Find in Mountain Array

#: 1095
Difficult: Hard
Tags: BS_Idx
link: https://leetcode.com/problems/find-in-mountain-array/
Priority: Low
Created time: June 27, 2023 11:21 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

*(This problem is an **interactive problem**.)*

You may recall that an array `arr` is a **mountain array** if and only if:

- `arr.length >= 3`
- There exists some `i` with `0 < i < arr.length - 1` such that:
    - `arr[0] < arr[1] < ... < arr[i - 1] < arr[i]`
    - `arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`

Given a mountain array `mountainArr`, return the **minimum** `index` such that `mountainArr.get(index) == target`. If such an `index` does not exist, return `-1`.

**You cannot access the mountain array directly.** You may only access the array using a `MountainArray` interface:

- `MountainArray.get(k)` returns the element of the array at index `k` (0-indexed).
- `MountainArray.length()` returns the length of the array.

Submissions making more than `100` calls to `MountainArray.get` will be judged *Wrong Answer*. Also, any solutions that attempt to circumvent the judge will result in disqualification.

**Example 1:**

```
Input: array = [1,2,3,4,5,3,1], target = 3
Output: 2
Explanation: 3 exists in the array, at index=2 and index=5. Return the minimum index, which is 2.
```

**Example 2:**

```
Input: array = [0,1,2,4,2,1], target = 3
Output: -1
Explanation: 3 does not exist inthe array, so we return -1.

```

**Constraints:**

- `3 <= mountain_arr.length() <= 104`
- `0 <= target <= 109`
- `0 <= mountain_arr.get(index) <= 109`

```cpp
/**
 * // This is the MountainArray's API interface.
 * // You should not implement it, or speculate about its implementation
 * class MountainArray {
 *   public:
 *     int get(int index);
 *     int length();
 * };
 */

class Solution {
public:
    int findInMountainArray(int target, MountainArray &nums) {
        int n=nums.length();
        int l=0,r=n-1;
        while(l<r){
            int m=l+(r-l)/2;
            if(nums.get(m)<nums.get(m+1)) l=m+1;
            else r=m;
        }
        int mountIdx=l;

        l=0, r=mountIdx;
        while(l<r){
            int m=l+(r-l)/2;
            if(nums.get(m)<target) l=m+1;
            else r=m;
        }
        if(nums.get(l)==target) return l;

        l=mountIdx, r=n-1;
        while(l<r){
            int m=l+(r-l)/2;
            if(nums.get(m)>target) l=m+1;
            else r=m;
        }
        if(nums.get(l)==target) return l;
        return -1;
    }
};

/*
find the peak: find the first dec
left side: bs_idx:
right side: bs_idx:
*
```