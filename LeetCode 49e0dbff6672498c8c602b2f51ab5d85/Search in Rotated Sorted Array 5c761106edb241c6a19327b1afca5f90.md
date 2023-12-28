# Search in Rotated Sorted Array

#: 33
Difficult: Medium
Tags: BS_Idx
link: https://leetcode.com/problems/search-in-rotated-sorted-array/
Priority: Low
Created time: July 8, 2022 12:32 PM
Last edited time: October 12, 2023 2:57 PM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, jiuzhang, leetcode_daily, 算初BS

There is an integer array `nums` sorted in ascending order (with **distinct** values).

Prior to being passed to your function, `nums` is **possibly rotated** at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.

Given the array `nums` **after** the possible rotation and an integer `target`, return *the index of* `target` *if it is in* `nums`*, or* `-1` *if it is not in* `nums`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4

```

**Example 2:**

```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1

```

**Example 3:**

```
Input: nums = [1], target = 0
Output: -1

```

**Constraints:**

- `1 <= nums.length <= 5000`
- `104 <= nums[i] <= 104`
- All values of `nums` are **unique**.
- `nums` is an ascending array that is possibly rotated.
- `104 <= target <= 104`

**注意m也是偏左的，可以安全的和r比较**

Show how to cut useless part, and only cut left or right(divide just 2 parts)

```jsx
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n=nums.size(); if(n==0) {return -1;}
        int l=0,r=n-1;
        
        while(l<r){
            int m=l+(r-l)/2;
            
            if(nums[l]<=nums[m]){
                if(nums[m]<target || target<nums[l]) {l=m+1;}//if not qualify
                else {r=m;}
            }
            else{
                if(nums[m]<target && target<nums[l]) {l=m+1;}//if not qualify
                else {r=m;}
            }
        }
        
        //find element
        if(nums[l]==target) return l;
        return -1;
    }
};
```

```cpp
/*class Solution {*/
/*    int search(vector<int> &A, int target) {*/
/*        if(A.empty()) return -1;*/
/*        int l=0,r=A.size()-1;*/
/*        while(l<r){*/
/*            int m=l+(r-l)/2;*/
/*            if(A[m]<A[r]){*/
/*                if(A[m]<target && target<=A[r]) l=m+1;*/
/*                else r=m;*/
/*            }*/
/*            else{*/
/*                if(target>A[m] || target <A[l]) l=m+1;*/
/*                else r=m;*/
/*            }*/
/*        }*/
/*        if(A[l]!=target) return -1;*/
/*        return l;*/
/*    }*/
/*};*/

class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n=nums.size(); if(n==0) {return -1;}
        int l=0,r=n-1;
        while(l<r){
            int m=l+(r-l)/2;
            if(nums[l]<nums[r]){
                if(nums[m]<target){l=m+1;}
                else{r=m;}
            }
            else if(nums[m]<nums[r]){
                if(nums[m]<target && target<=nums[r]){l=m+1;}
                else{r=m;}
            }
            else{
                if(nums[m]<target || target<=nums[r]){l=m+1;}
                else{r=m;}
            }
        }
        if(nums[l]==target){return l;}
        return -1;
    }
};
```