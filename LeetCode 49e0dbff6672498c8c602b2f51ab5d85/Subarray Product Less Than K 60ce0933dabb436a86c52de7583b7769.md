# Subarray Product Less Than K

#: 713
Difficult: Medium
Tags: 2pt_SameDir_maxWin_chkValidAftR, Array
link: Given an array of integers nums and an integer k, return the number of contiguous subarrays where the product of all the elements in the subarray is strictly less than k.
Priority: Medium
Status: toSummarize
Created time: May 12, 2023 2:43 AM
Last edited time: December 8, 2023 6:57 PM
source: facebookFreq, wisdompeak

Given an array of integers `nums` and an integer `k`, return *the number of contiguous subarrays where the product of all the elements in the subarray is strictly less than* `k`.

**Example 1:**

```
Input: nums = [10,5,2,6], k = 100
Output: 8
Explanation: The 8 subarrays that have product less than 100 are:
[10], [5], [2], [6], [10, 5], [5, 2], [2, 6], [5, 2, 6]
Note that [10, 5, 2] is not included as the product of 100 is not strictly less than k.

```

**Example 2:**

```
Input: nums = [1,2,3], k = 0
Output: 0

```

**Constraints:**

- `1 <= nums.length <= 3 * 104`
- `1 <= nums[i] <= 1000`
- `0 <= k <= 106`

comment: to summarize the ≤k similar questions, 

```cpp
class Solution {
public:
    int numSubarrayProductLessThanK(vector<int>& nums, int k) {
        if(k==0) return 0;
        int res=0;
        int n=nums.size();// if(n==0) {return 0;}
        int prod=1;
        for(int l=0,r=0;r<n;r++){//l should be at most 1 postion right of r, when prod ==1, handle k==0
            //add r
            prod*=nums[r];
            //trim l until valid
            while(prod>=k && l<r+1){
                prod/=nums[l];
                l++;
            }
            //update res
            res+=r-l+1;
        }
        return res;
    }
};

/*
return: num of subarr, that product of all elems < k
================================
nums:
    len: 1~3e4
    val: 1~1000
k: 0~1e6
================================
prod can't dec <= (val>=1)
the larger the len, the bigger the prod

largest sliding win - l~r, that valid prod < k
then res+=(r-l+1);//valid subarr end with r

*/
```