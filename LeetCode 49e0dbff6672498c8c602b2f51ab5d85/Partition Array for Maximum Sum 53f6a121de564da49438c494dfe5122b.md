# Partition Array for Maximum Sum

#: 1043
Difficult: Medium
Tags: Array, dp-Pyr, dp-TimeSeqFromLastX
link: https://leetcode.com/problems/partition-array-for-maximum-sum/
Priority: Low
Created time: June 28, 2023 12:44 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given an integer array `arr`, partition the array into (contiguous) subarrays of length **at most** `k`. After partitioning, each subarray has their values changed to become the maximum value of that subarray.

Return *the largest sum of the given array after partitioning. Test cases are generated so that the answer fits in a **32-bit** integer.*

**Example 1:**

```
Input: arr = [1,15,7,9,2,5,10], k = 3
Output: 84
Explanation: arr becomes [15,15,15,9,10,10,10]

```

**Example 2:**

```
Input: arr = [1,4,1,5,7,3,6,1,9,9,3], k = 4
Output: 83

```

**Example 3:**

```
Input: arr = [1], k = 1
Output: 1

```

**Constraints:**

- `1 <= arr.length <= 500`
- `0 <= arr[i] <= 109`
- `1 <= k <= arr.length`

```cpp
class Solution {
public:
    int maxSumAfterPartitioning(vector<int>& arr, int k) {
        int n=arr.size();
        vector<int> dp(n,0);
        for(int i=0; i<n; i++){
            for(int len=1; len<=k && i-len+1>=0; len++){
                int tmp=len * (*max_element(arr.begin()+i-len+1, arr.begin()+i+1));//to use dp pyr to preprocess
                dp[i] = max(dp[i], (i-len>=0?dp[i-len]:0) + tmp);
            }
        }
        return dp.back();
    }
};

/*
f idx
f i = [i,i+len-1] + f[i+len], len: 1~k
return f[0]
===>
f i = f[i-len] + [i-len+1,i], len: 1~k
      i-len<0:0.  i-len+1>=0
return f.back
*/
```