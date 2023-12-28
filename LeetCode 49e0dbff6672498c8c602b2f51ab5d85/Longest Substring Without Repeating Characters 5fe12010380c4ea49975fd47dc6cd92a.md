# Longest Substring Without Repeating Characters

#: 3
Difficult: Medium
Tags: 2pt_SameDir_maxWin_chkValidBfrR, Hash, Sliding Window
link: https://leetcode.com/problems/longest-substring-without-repeating-characters/
Priority: Medium
Created time: August 20, 2022 1:35 PM
Last edited time: October 22, 2023 2:11 PM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, jiuzhang, 算高TwoPointers&PQ

Given a string `s`, find the length of the **longest substring** without repeating characters.

**Example 1:**

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.

```

**Example 2:**

```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.

```

**Example 3:**

```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

```

**Constraints:**

- `0 <= s.length <= 5 * 104`
- `s` consists of English letters, digits, symbols and spaces.

**Solution:**

hast_set

```jsx
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int n=s.size(); if(n==0) return 0;
        int res=0;
        unordered_set<char> st;
        for(int l=0,r=0;r<n;r++){
            while(st.count(s[r]) && l<=r){
                if(st.count(s[l])) st.erase(s[l]);
                l++;
            }
            st.insert(s[r]);
            res=max(res,r-l+1);
        }
        return res;
    }
};
```

hash_map记录上次访问

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int n=s.size(); if(n==0) return 0;
        unordered_map<char, int> mp; //char_lastSeenIdx
        int res=0;
        //l,r, r keep moving
        for(int l=0,r=0;r<n;r++){
            //r is new one
            //if invalid, update l
            if(mp.count(s[r]) && mp[s[r]]>=l){
                l=mp[s[r]]+1;
            }
            //update mp
            mp[s[r]]=r;
            //update res
            res=max(res,r-l+1);
        }
        return res;
    }
};
//verify valid or not when new one coming, can't record status
```