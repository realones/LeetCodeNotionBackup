# Longest Arithmetic Subsequence of Given Difference

#: 1218
Difficult: Medium
Tags: DP, Hash
link: https://leetcode.com/problems/longest-arithmetic-subsequence-of-given-difference
Priority: High
Status: No Idea At First
Created time: June 27, 2023 9:24 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, leetcode_daily

Given an integer array `arr` and an integer `difference`, return the length of the longest subsequence in `arr` which is an arithmetic sequence such that the difference between adjacent elements in the subsequence equals `difference`.

A **subsequence** is a sequence that can be derived from `arr` by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

```
Input: arr = [1,2,3,4], difference = 1
Output: 4
Explanation:The longest arithmetic subsequence is [1,2,3,4].
```

**Example 2:**

```
Input: arr = [1,3,5,7], difference = 1
Output: 1
Explanation:The longest arithmetic subsequence is any single element.

```

**Example 3:**

```
Input: arr = [1,5,7,8,5,3,4,2,1], difference = -2
Output: 4
Explanation:The longest arithmetic subsequence is [7,5,3,1].

```

**Constraints:**

- `1 <= arr.length <= 1e5`
- `-1e4 <= arr[i], difference <= 1e4`

Solution:

一开始想到dp认为是n^2 complexity，没想到可以用mp

```cpp
class Solution {
public:
    int longestSubsequence(vector<int>& nums, int difference) {
        int n=nums.size();//n:1~1e5, val & diff:-1e4~1e4
        unordered_map<int,int> mp;
        int res=1;
        for(auto x: nums){
            if(mp.count(x-difference)){
                mp[x]=max(mp[x],mp[x-difference]+1);
            }
            mp[x]=max(mp[x],1);
            res=max(res,mp[x]);
        }
        return res;
    }
};

/*
brute force: find all subset 2^n, then check if valid

longest with cur (cur++): n^2
    with mp[val,longestLen] -> n
*
```