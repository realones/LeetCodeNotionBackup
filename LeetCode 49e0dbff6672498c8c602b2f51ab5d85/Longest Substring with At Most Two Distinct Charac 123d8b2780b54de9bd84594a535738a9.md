# Longest Substring with At Most Two Distinct Characters

#: 159
Difficult: Medium
Tags: Hash, Sliding Window, string
link: https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters
Priority: Pass
Created time: November 20, 2023 2:03 PM
Last edited time: November 20, 2023 2:04 PM
source: LC_Premium_Algo_100

Given a string `s`, return *the length of the longest*

*substring*

*that contains at most **two distinct characters***

.

**Example 1:**

```
Input: s = "eceba"
Output: 3
Explanation: The substring is "ece" which its length is 3.

```

**Example 2:**

```
Input: s = "ccaabbb"
Output: 5
Explanation: The substring is "aabbb" which its length is 5.

```

**Constraints:**

- `1 <= s.length <= 105`
- `s` consists of English letters.

```cpp
class Solution {
public:
    int lengthOfLongestSubstringTwoDistinct(string s) {
        unordered_map<char,int> mp;//char_freq
        int n=s.size();
        int cnt=0;
        int res=0;
        for(int l=0, r=0; r<n; r++){
            mp[s[r]]++;
            if(mp[s[r]]==1) cnt++;
            //while invalid , trime l
            while(cnt>2){
                mp[s[l]]--;
                if(mp[s[l]]==0) cnt--;
                l++;
            }
            //update res
            res=max(res,r-l+1);
        }
        return res;
    }
};
```