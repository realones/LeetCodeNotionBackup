# Number of Substrings Containing All Three Characters

#: 1358
Difficult: Medium
Tags: 2pt_SameDir_minWin_chkValidAftR, Hash, string
link: https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/
Priority: High
Status: HardToImplement, No Idea At First
Created time: December 8, 2023 3:08 PM
Last edited time: December 8, 2023 3:20 PM
source: facebookFreq

Given a string `s` consisting only of characters *a*, *b* and *c*.

Return the number of substrings containing **at least** one occurrence of all these characters *a*, *b* and *c*.

**Example 1:**

```
Input: s = "abcabc"
Output: 10
Explanation: The substrings containing at least one occurrence of the charactersa,b andc are "abc", "abca", "abcab", "abcabc", "bca", "bcab", "bcabc", "cab", "cabc"and "abc"(again).
```

**Example 2:**

```
Input: s = "aaacb"
Output: 3
Explanation: The substrings containing at least one occurrence of the charactersa,b andc are "aaacb", "aacb"and "acb".
```

**Example 3:**

```
Input: s = "abc"
Output: 1

```

**Constraints:**

- `3 <= s.length <= 5 x 10^4`
- `s` only consists of *a*, *b* or *c* characters.

Solution lee215

```cpp
class Solution {
public:
    int numberOfSubstrings(string s) {
        int n=s.size();
        int res=0;
        unordered_map<char,int> mp;//to use three int
        int mpSz=0;
        for(int l=0,r=0; r<n ;r++){
            //add r
            mp[s[r]]++;
            if(mp[s[r]]==1) mpSz++;
            //trim l
            while(mpSz==3){
                mp[s[l]]--;
                if(mp[s[l]]==0) mpSz--;
                l++;
            }
            //update res
            res+=l;
        }
        return res;
    }
};

/*
find smallest sliding window[l,r] contains at least one a,b,c
then all substr expanding with left [0...l : r], r is fixed, add to result
*/
```

Solution mine:

```cpp
class Solution {
public:
    int numberOfSubstrings(string s) {
        int n=s.size();
        int res=0;
        unordered_map<char,int> mp;//to use three int
        int mpSz=0;
        for(int l=0,r=0; r<n ;r++){
            //add r
            mp[s[r]]++;
            if(mp[s[r]]==1) mpSz++;
            //trim l and update res when valid
            bool valid=(mpSz==3);
            if(valid){
                while(mpSz==3){
                    mp[s[l]]--;
                    if(mp[s[l]]==0) mpSz--;
                    l++;
                }
                l--;
                mp[s[l]]++;
                mpSz++;
                res+=l+1;
            }
        }
        return res;
    }
};

/*
find smallest sliding window[l,r] contains at least one a,b,c
then all substr expanding with left [0...l : r], r is fixed, add to result
*/
```