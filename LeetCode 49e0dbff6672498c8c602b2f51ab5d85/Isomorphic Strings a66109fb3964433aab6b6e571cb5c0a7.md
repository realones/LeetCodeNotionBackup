# Isomorphic Strings

#: 205
Difficult: Easy
Tags: Hash, string
link: https://leetcode.com/problems/isomorphic-strings/
Priority: Low
Created time: October 10, 2022 2:52 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150, googleFreq

Given two strings `s` and `t`, *determine if they are isomorphic*.

Two strings `s` and `t` are isomorphic if the characters in `s` can be replaced to get `t`.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character, but a character may map to itself.

**Example 1:**

```
Input: s = "egg", t = "add"
Output: true

```

**Example 2:**

```
Input: s = "foo", t = "bar"
Output: false

```

**Example 3:**

```
Input: s = "paper", t = "title"
Output: true

```

**Constraints:**

- `1 <= s.length <= 5 * 104`
- `t.length == s.length`
- `s` and `t` consist of any valid ascii character.

1:1 map

```cpp
class Solution {
public:
    bool isIsomorphic(string s, string t) {
        if(s.size()!=t.size()) return false;
        int n=s.size(); if(n==0) return true;
        vector<int> si_ti(256,-1);
        vector<int> ti_si(256,-1);
        for(int i=0; i<n; i++){
            int si=s[i], ti=t[i];
            if(si_ti[si]==-1 && ti_si[ti]==-1) {
                si_ti[si]=ti;
                ti_si[ti]=si;
            }
            else if(si_ti[si]!=ti || ti_si[ti]!=si){return false;}
        }
        return true;
    }
};
```