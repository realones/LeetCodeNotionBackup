# Find All Anagrams in a String

#: 438
Difficult: Medium
Tags: 2pt_SameDir_sameSpeed, Hash, Sliding Window
link: https://leetcode.com/problems/find-all-anagrams-in-a-string/
Priority: Medium
Created time: October 16, 2022 6:17 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, googleFreq

Given two strings `s` and `p`, return *an array of all the start indices of* `p`*'s anagrams in* `s`. You may return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**

```
Input: s = "cbaebabacd", p = "abc"
Output: [0,6]
Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".

```

**Example 2:**

```
Input: s = "abab", p = "ab"
Output: [0,1,2]
Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".

```

**Constraints:**

- `1 <= s.length, p.length <= 3 * 104`
- `s` and `p` consist of lowercase English letters.

```cpp
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        vector<int> res;
        int n=s.size(); if(n==0) return res;
        int k=p.size(); if(k==0 || k>n) return res;
        unordered_map<char,int> mp;
        for(auto c:p){mp[c]++;}
        int sz=mp.size();
        
        for(int i=0;i<n;i++){
            //add cur char, neutralize char in map 
            if(mp.count(s[i])) {
                mp[s[i]]--;
                if(mp[s[i]]==0) sz--;
            }
            //if >k-1, trim tail, add cnt to map
            if(i>k-1){
                if(mp.count(s[i-k])) {
                    mp[s[i-k]]++;
                    if(mp[s[i-k]]==1) sz++;
                }
            }
            //if map == 00000, add res
            if(sz==0){res.push_back(i-k+1);}
        }        
        
        
        return res;
    }
};
```