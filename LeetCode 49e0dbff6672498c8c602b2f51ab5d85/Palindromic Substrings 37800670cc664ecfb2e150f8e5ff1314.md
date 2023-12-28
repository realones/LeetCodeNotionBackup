# Palindromic Substrings

#: 647
Difficult: Medium
Tags: DP, string
link: https://leetcode.com/problems/palindromic-substrings/description/
Priority: Low
Created time: November 23, 2023 2:57 AM
Last edited time: November 23, 2023 2:58 AM
source: facebookFreq

Given a string `s`, return *the number of **palindromic substrings** in it*.

A string is a **palindrome** when it reads the same backward as forward.

A **substring** is a contiguous sequence of characters within the string.

**Example 1:**

```
Input: s = "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".

```

**Example 2:**

```
Input: s = "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".

```

**Constraints:**

- `1 <= s.length <= 1000`
- `s` consists of lowercase English letters.

```cpp
class Solution {
public:
    int countSubstrings(string s) {
        int n=s.size();
        int cnt=0;
        //1 core
        for(int i=0; i<n; i++){
            int l=i,r=i;
            while(l>=0 && r<n && s[l]==s[r]){
                cnt++;
                l--,r++;
            }
        }
        //2 core: i&i+1
        for(int i=0; i<n-1; i++){
            int l=i,r=i+1;
            while(l>=0 && r<n && s[l]==s[r]){
                cnt++;
                l--,r++;
            }
        }
        return cnt;
    }
};

/*
for ij 1000*1000
    if palim: 1000
================================
for each core, expand
    n*n
*/
```