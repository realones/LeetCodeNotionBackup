# Maximum OR

#: 2680
Difficult: Medium
Tags: Array, Bit Manipulation, Greedy, Math, Prefix
link: https://leetcode.com/problems/maximum-or/
Priority: High
Status: No Idea At First
Created time: June 16, 2023 5:36 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given a **0-indexed** integer array `nums` of length `n` and an integer `k`. In an operation, you can choose an element and multiply it by `2`.

Return *the maximum possible value of* `nums[0] | nums[1] | ... | nums[n - 1]` *that can be obtained after applying the operation on nums at most* `k` *times*.

Note that `a | b` denotes the **bitwise or** between two integers `a` and `b`.

**Example 1:**

```
Input: nums = [12,9], k = 1
Output: 30
Explanation: If we apply the operation to index 1, our new array nums will be equal to [12,18]. Thus, we return the bitwise or of 12 and 18, which is 30.

```

**Example 2:**

```
Input: nums = [8,1,2], k = 2
Output: 35
Explanation: If we apply the operation twice on index 0, we yield a new array of [32,1,2]. Thus, we return 32|1|2 = 35.

```

**Constraints:**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 109`
- `1 <= k <= 15`

Solution:

Hint 1: The optimal solution should apply all the k operations on a single number.
Hint 2: Calculate the prefix or and the suffix or and perform k operations over each element, and maximize the answer.

```cpp
using ll=long long;

class Solution {
public:
    long long maximumOr(vector<int>& nums, int k) {//k:15
        int n=nums.size(); if(n==0) {return 0;}//1e5
        vector<int> prefix = nums;
        for(int i=1; i<n; i++){
            prefix[i] |= prefix[i-1];
        }
        vector<int> suffix = nums;
        for(int i=n-2; i>=0; i--){
            suffix[i] |= suffix[i+1];
        }
        
        ll res=0;
        for(int i=0; i<n; i++){
            ll p = i-1>=0?prefix[i-1]:0;
            ll s = i+1<n?suffix[i+1]:0;
            res=max(res, ((ll)nums[i]<<k)| p| s);
        }
        return res;
    }
};
```