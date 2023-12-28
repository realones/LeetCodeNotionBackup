# Permutation in String

#: 567
Difficult: Medium
Tags: 2pt_SameDir_sameSpeed, Hash, Sliding Window, string
link: https://leetcode.com/problems/permutation-in-string/description/
Priority: Low
Created time: June 27, 2023 9:27 PM
Last edited time: December 22, 2023 1:52 PM
source: HuaHua, googleFreq

Given two strings `s1` and `s2`, return `true` *if* `s2` *contains a permutation of* `s1`*, or* `false` *otherwise*.

In other words, return `true` if one of `s1`'s permutations is the substring of `s2`.

**Example 1:**

```
Input: s1 = "ab", s2 = "eidbaooo"
Output: true
Explanation: s2 contains one permutation of s1 ("ba").

```

**Example 2:**

```
Input: s1 = "ab", s2 = "eidboaoo"
Output: false

```

**Constraints:**

- `1 <= s1.length, s2.length <= 104`
- `s1` and `s2` consist of lowercase English letters.

```cpp
class Solution {
public:
    bool checkInclusion(string s1, string s2) {
        int k=s1.size();
        if(s1.size()>s2.size()) return false;
        unordered_map<int,int> mp;
        for(auto c: s1){
            mp[c]++;
        }
        int mpSz=mp.size();

        for(int i=0;i<s2.size();i++){
            //add i
            if(mp.count(s2[i])) {
                mp[s2[i]]--;
                if(mp[s2[i]]==0) mpSz--;
            }
            //trim l
            if(i>k-1){
                if(mp.count(s2[i-k])) {
                    mp[s2[i-k]]++;
                    if(mp[s2[i-k]]==1) mpSz++;
                }
            }
            //update res
            if(i>=k-1){
                if(mpSz==0) return true;
            }
        }
        return false;
    }
};
```