# Maximum Average Subarray I

#: 643
Difficult: Easy
Tags: Array, Sliding Window
link: https://leetcode.com/problems/maximum-average-subarray-i
Priority: Low
Created time: July 2, 2023 1:04 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_75

You are given an integer array `nums` consisting of `n` elements, and an integer `k`.

Find a contiguous subarray whose **length is equal to** `k` that has the maximum average value and return *this value*. Any answer with a calculation error less than `10-5` will be accepted.

**Example 1:**

```
Input: nums = [1,12,-5,-6,50,3], k = 4
Output: 12.75000
Explanation: Maximum average is (12 - 5 - 6 + 50) / 4 = 51 / 4 = 12.75

```

**Example 2:**

```
Input: nums = [5], k = 1
Output: 5.00000

```

**Constraints:**

- `n == nums.length`
- `1 <= k <= n <= 105`
- `104 <= nums[i] <= 104`

```cpp
using ll=long long;
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        int n=nums.size(); if(n==0) {return 0;}
        double res=INT_MIN;
        ll sum=0;
        for(int i=0; i<n; i++){
            sum+=nums[i];
            if(i>k-1) sum-=nums[i-k];
            if(i>=k-1) res=max(res,sum*1.0/k);
        }
        return res;
    }
};

/*
subarr len = k, max ave
sliding window
*/
```