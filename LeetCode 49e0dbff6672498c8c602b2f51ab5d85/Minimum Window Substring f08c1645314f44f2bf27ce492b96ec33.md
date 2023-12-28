# Minimum Window Substring

#: 76
Difficult: Hard
Tags: 2pt_SameDir_minWin_chkValidAftR, Sliding Window
link: https://leetcode.com/problems/minimum-window-substring/
Priority: Medium
Created time: August 20, 2022 1:35 PM
Last edited time: October 22, 2023 6:34 PM
practicedTimes: 1
source: LC_Top_Interview_150, jiuzhang, 算高TwoPointers&PQ

Given two strings `s` and `t` of lengths `m` and `n` respectively, return *the **minimum window substring** of* `s` *such that every character in* `t` *(**including duplicates**) is included in the window. If there is no such substring, return the empty string* `""`*.*

The testcases will be generated such that the answer is **unique**.

A **substring** is a contiguous sequence of characters within the string.

**Example 1:**

```
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.

```

**Example 2:**

```
Input: s = "a", t = "a"
Output: "a"
Explanation: The entire string s is the minimum window.

```

**Example 3:**

```
Input: s = "a", t = "aa"
Output: ""
Explanation: Both 'a's from t must be included in the window.
Since the largest window of s only has one 'a', return empty string.

```

**Constraints:**

- `m == s.length`
- `n == t.length`
- `1 <= m, n <= 105`
- `s` and `t` consist of uppercase and lowercase English letters.

**Follow up:**

Could you find an algorithm that runs in

```
O(m + n)
```

time?

```cpp
class Solution {
public:
    string minWindow(string s, string t) {
        if(s=="" || t=="" || s.size()<t.size()) return "";
        
        string res="";
        int resLen=INT_MAX;
        unordered_map<char,int> mp; //char_cnt
        for(auto c:t){mp[c]++;}
        int mpSz=mp.size();
        
        for(int l=0,r=0;r<s.size();r++){
            //update status with nums[r]
            if(mp.count(s[r])){
                mp[s[r]]--;
                if(mp[s[r]]==0) mpSz--;
            }
            //if/while valid, update l till invalid
            while(mpSz==0){
                //update res
                if(r-l+1<resLen){
                    resLen=r-l+1;
                    res=s.substr(l,r-l+1);
                }
                //update status with removing nums[l]
                if(mp.count(s[l])){
                    mp[s[l]]++;
                    if(mp[s[l]]==1) mpSz++;
                }
                //move l to right by 1 or many
                l++;
            }
        }
        
        return res;
    }
};
```