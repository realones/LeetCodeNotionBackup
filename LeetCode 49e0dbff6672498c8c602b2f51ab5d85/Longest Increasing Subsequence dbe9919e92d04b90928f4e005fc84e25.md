# Longest Increasing Subsequence

#: 300
Difficult: Medium
Tags: Array, BS_todo, DP, GoForth, PatienceSort, UsePreResults, dp-TimeSeqFromLastX
link: https://leetcode.com/problems/longest-increasing-subsequence/
Priority: High
Status: More Solution, toRevisit
Created time: August 18, 2022 11:47 AM
Last edited time: October 22, 2023 12:20 AM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, jiuzhang, wp动归套路, 算初DP
related: Number of Longest Increasing Subsequence (Number%20of%20Longest%20Increasing%20Subsequence%20002a704f8f96414397bb7945631700c7.md), Longest String Chain (Longest%20String%20Chain%20454f01bd38334ac2addfd34384e6b23b.md)

Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

A **subsequence** is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, `[3,6,2,7]` is a subsequence of the array `[0,3,1,6,2,2,7]`.

**Example 1:**

```
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.

```

**Example 2:**

```
Input: nums = [0,1,0,3,2,3]
Output: 4

```

**Example 3:**

```
Input: nums = [7,7,7,7,7,7,7]
Output: 1

```

**Constraints:**

- `1 <= nums.length <= 2500`
- `104 <= nums[i] <= 104`

**Follow up:** Can you come up with an algorithm that runs in `O(n log(n))` time complexity?

Solution 1 : PatientSort + BS

T: nlogn

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n=nums.size(); if(n==0) {return 0;}
        vector<int> inc;
        for(auto num: nums){
            auto pos=lower_bound(inc.begin(), inc.end(), num);
            if(pos==inc.end()){inc.push_back(num);}
            else{
                *pos=num;
            }
        }
        return inc.size();
    }
};
```

Solution 2: order map

T: nlogn

Solution 3:

please check following 2 other solutions, also check their discussions for better explanation,.

[https://leetcode.com/problems/longest-increasing-subsequence/discuss/74824/JavaPython-Binary-search-O(nlogn)-time-with-explanation](https://leetcode.com/problems/longest-increasing-subsequence/discuss/74824/JavaPython-Binary-search-O(nlogn)-time-with-explanation)

[https://leetcode.com/problems/longest-increasing-subsequence/discuss/74848/9-lines-C%2B%2B-code-with-O(NlogN)-complexity](https://leetcode.com/problems/longest-increasing-subsequence/discuss/74848/9-lines-C%2B%2B-code-with-O(NlogN)-complexity)

- 将n个数看做n个木桩，目的是从某个木桩出发，从前向后，从低往高，看做多能踩多少个木桩。 （*九章解释。。一般般*）
- state: f[i] 表示(从任意某个木桩)跳到第i个木桩，最多踩过多少根木桩
- function: f[i] = max{f[j] + 1}, j必须满足 j < i && nums[j] < nums[i]
- initialize: f[0..n-1] = 1
- answer: max{f[0..n-1]}

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n=nums.size(); if(n==0) return 0;
        int res=1;
        vector<int> dp(n,1);
        for(int k=1;k<n;k++){
            for(int i=0;i<k;i++){
                if(nums[i]>=nums[k]) continue;
                dp[k]=max(dp[k],dp[i]+1);    
            }
						res=max(res,dp[k]);
        }
        return res;
    }
};
```