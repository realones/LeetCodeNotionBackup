# Is Subsequence

#: 392
Difficult: Easy
Tags: 2pt, DP, string
link: https://leetcode.com/problems/is-subsequence/
Priority: Medium
Status: More Solution, toSummarize
Created time: May 31, 2023 6:36 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_75, LC_Top_Interview_150

Given two strings `s` and `t`, return `true` *if* `s` *is a **subsequence** of* `t`*, or* `false` *otherwise*.

A **subsequence** of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).

**Example 1:**

```
Input: s = "abc", t = "ahbgdc"
Output: true

```

**Example 2:**

```
Input: s = "axc", t = "ahbgdc"
Output: false

```

**Constraints:**

- `0 <= s.length <= 100`
- `0 <= t.length <= 104`
- `s` and `t` consist only of lowercase English letters.

**Follow up:**

Suppose there are lots of incoming s , say s1, s2, ..., sk where k >= 1e9 , and you want to check one by one to see if t has its subsequence. In this scenario, how would you change your code?

solution: todo, summarize this for loop

hmm, just maintain idx for s, and loop thought t should be fine

```cpp
class Solution {
public:
    bool isSubsequence(string s, string t) {
        if(s.empty()) return true;
        if(t.empty()) return false;
        int i=0,j=0;
        for(;i<s.size() && j<t.size();i++,j++){
            while(j<t.size() && s[i]!=t[j]){
                j++;
            }
            if(j==t.size()) return false;
        }
        if(i<s.size() && j==t.size()) return false;
        return true;
    }
};
```