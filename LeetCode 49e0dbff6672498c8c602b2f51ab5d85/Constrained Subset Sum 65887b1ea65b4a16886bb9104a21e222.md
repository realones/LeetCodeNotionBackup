# Constrained Subset Sum

#: 1425
Difficult: Hard
Tags: Array, Sliding Window, deque, dp-TimeSeqFromLastX, monotonic
link: https://leetcode.com/problems/constrained-subsequence-sum/
Priority: High
Status: HardToImplement, No Idea At First
Created time: June 28, 2023 12:35 AM
Last edited time: October 21, 2023 1:02 AM
source: HuaHua, leetcode_daily

Given an integer array `nums` and an integer `k`, return the maximum sum of a **non-empty** subsequence of that array such that for every two **consecutive** integers in the subsequence, `nums[i]` and `nums[j]`, where `i < j`, the condition `j - i <= k` is satisfied.

A *subsequence* of an array is obtained by deleting some number of elements (can be zero) from the array, leaving the remaining elements in their original order.

**Example 1:**

```
Input: nums = [10,2,-10,5,20], k = 2
Output: 37
Explanation: The subsequence is [10, 2, 5, 20].

```

**Example 2:**

```
Input: nums = [-1,-2,-3], k = 1
Output: -1
Explanation: The subsequence must be non-empty, so we choose the largest number.

```

**Example 3:**

```
Input: nums = [10,-2,-10,-5,20], k = 2
Output: 23
Explanation: The subsequence is [10, -2, -5, 20].

```

**Constraints:**

- `1 <= k <= nums.length <= 105`
- `104 <= nums[i] <= 104`

Solution: dp 1d with pre k

```cpp
class Solution {
public:
    int constrainedSubsetSum(vector<int>& nums, int k) {
        nums.insert(nums.begin(),0);//make corner case handling easier
        int n=nums.size();
        vector<int> dp(n,INT_MIN);
        dp[0]=0;
        for(int i=1; i<n; i++){
            dp[i]=nums[i];
            for(int l=i-1;i-l<=k && l>=0;l--){
                dp[i] = max(dp[i], dp[l]+nums[i]);
            }
        }
        return *max_element(dp.begin()+1,dp.end());
    }
};

/*
valid: for all consequtive num i and j, j-i<=k
return: max sum of valid subsequence 
================================
len: 1~1e5
k: 1~1e5
val: -1e4~1e4
================================
dp - T:O(nk)
dp[i]: solution for nums[0]~nums[i], nums[i] must in subseq
dp[i] max=
    dp[l] (l: i-1 as long as i-l<=k), if l<0, dp[l]=0
    + cur
init:
    handled by above corner case
return:
    max of dp[any]
================================
sliding window maximum - mono deque
*/
```

Solution: dp+dq_slidingWindow

```cpp
class Solution {
public:
    int constrainedSubsetSum(vector<int>& nums, int k) {
        nums.insert(nums.begin(),0);//make corner case handling easier
        int n=nums.size();
        vector<int> dp(n,INT_MIN);
        dp[0]=0;
        deque<int> q;
        q.push_back(0);
        for(int i=1; i<n; i++){
            if(i>k){
                if(dp[q.front()]==dp[i-k-1]) q.pop_front();
            }
            dp[i] = max(nums[i], dp[q.front()]+nums[i]);
            while(!q.empty() && dp[q.back()]<dp[i]) q.pop_back();
            q.push_back(i);
        }
        return *max_element(dp.begin()+1,dp.end());
    }
};

/*
valid: for all consequtive num i and j, j-i<=k
return: max sum of valid subsequence 
================================
len: 1~1e5
k: 1~1e5
val: -1e4~1e4
================================
dp - T:O(nk)
dp[i]: solution for nums[0]~nums[i], nums[i] must in subseq
dp[i] max=
    dp[l] (l: i-1 as long as i-l<=k), if l<0, dp[l]=0
    + cur
init:
    handled by above corner case
return:
    max of dp[any]
================================
sliding window maximum - mono deque
*/
```