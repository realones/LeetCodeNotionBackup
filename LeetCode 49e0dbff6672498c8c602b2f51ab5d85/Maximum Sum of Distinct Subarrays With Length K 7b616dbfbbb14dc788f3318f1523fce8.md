# Maximum Sum of Distinct Subarrays With Length K

#: 2461
Difficult: Medium
Tags: Array, Hash, Sliding Window
link: https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/
Priority: Low
Created time: November 16, 2023 12:43 AM
Last edited time: November 17, 2023 1:26 AM
practicedTimes: 1
source: AmazonFreq

You are given an integer array `nums` and an integer `k`. Find the maximum subarray sum of all the subarrays of `nums` that meet the following conditions:

- The length of the subarray is `k`, and
- All the elements of the subarray are **distinct**.

Return *the maximum subarray sum of all the subarrays that meet the conditions.* If no subarray meets the conditions, return `0`.

*A **subarray** is a contiguous non-empty sequence of elements within an array.*

**Example 1:**

```
Input: nums = [1,5,4,2,9,9,9], k = 3
Output: 15
Explanation: The subarrays of nums with length 3 are:
- [1,5,4] which meets the requirements and has a sum of 10.
- [5,4,2] which meets the requirements and has a sum of 11.
- [4,2,9] which meets the requirements and has a sum of 15.
- [2,9,9] which does not meet the requirements because the element 9 is repeated.
- [9,9,9] which does not meet the requirements because the element 9 is repeated.
We return 15 because it is the maximum subarray sum of all the subarrays that meet the conditions

```

**Example 2:**

```
Input: nums = [4,4,4], k = 3
Output: 0
Explanation: The subarrays of nums with length 3 are:
- [4,4,4] which does not meet the requirements because the element 4 is repeated.
We return 0 because no subarrays meet the conditions.

```

**Constraints:**

- `1 <= k <= nums.length <= 105`
- `1 <= nums[i] <= 105`

```cpp
using ll=long long;

class Solution {
public:
    long long maximumSubarraySum(vector<int>& nums, int k) {
        int n=nums.size();
        ll res=0;
        ll sum=0;
        unordered_map<int,int> mp;//int,freq
        int cnt=0;//dup char cnt
        for(int i=0;i<n;i++){
            //add nums[i]
            sum+=nums[i];
            mp[nums[i]]++;
            if(mp[nums[i]]==2) cnt++;
            //if i>=k, trim l
            if(i>=k) {
                sum-=nums[i-k];
                mp[nums[i-k]]--;
                if(mp[nums[i-k]]==1) cnt--;
            }
            //if i>=k-1, calcu when valid
            if(i>=k-1){
                if(cnt==0) res=max(res,sum);
            }
        }
        return res;
    }
};

/*
nums:
    len: 1~1e5
    val: 1~1e5
k: 1~nums.len
================================
slidingwindow:
    sz==k
    all elem distinct
================================
*/
```