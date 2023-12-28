# Longest Substring with At Most K Distinct Characters

#: 340
Difficult: Medium
Tags: 2pt_SameDir_maxWin_chkValidAftR
link: https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/
Priority: Medium
Created time: August 20, 2022 1:35 PM
Last edited time: October 22, 2023 2:24 PM
practicedTimes: 1
source: jiuzhang, 算高TwoPointers&PQ

Given a string `s` and an integer `k`, return *the length of the longest substring of* `s` *that contains at most* `k` ***distinct** characters*.

**Example 1:**

```
Input: s = "eceba", k = 2
Output: 3
Explanation: The substring is "ece" with length 3.
```

**Example 2:**

```
Input: s = "aa", k = 1
Output: 2
Explanation: The substring is "aa" with length 2.

```

**Constraints:**

- `1 <= s.length <= 5 * 104`
- `0 <= k <= 50`

```cpp
class Solution {
public:
    int lengthOfLongestSubstringKDistinct(string s, int k) {
        int n=s.size();
        if(n==0 || k==0) return 0;
        unordered_map<char,int> mp;//char_cnt
        int mpSz=0;
        int res=0;
        
        for(int l=0,r=0;r<n;r++){
            //update status with r
            mp[s[r]]++;
            if(mp[s[r]]==1) mpSz++;
            //while/if invalid, move l
            while(mpSz>k){
                mp[s[l]]--;
                if(mp[s[l]]==0) mpSz--;
                l++;
            }
            //update res
            res=max(res,r-l+1);
        }
        return res;
    }
};
```