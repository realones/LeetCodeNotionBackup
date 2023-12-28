# Longest Repeating Substring

#: 1062
Difficult: Medium
Tags: BS_todo, DP, Hash, Prefix, RollingHash, string
link: https://leetcode.com/problems/longest-repeating-substring/description/
Priority: High
Status: More Solution, No Idea At First, checkBetterSolution
Created time: December 24, 2023 1:50 AM
Last edited time: December 24, 2023 1:59 AM
source: googleFreq

Given a string `s`, return *the length of the longest repeating substrings*. If no repeating substring exists, return `0`.

**Example 1:**

```
Input: s = "abcd"
Output: 0
Explanation:There is no repeating substring.

```

**Example 2:**

```
Input: s = "abbaba"
Output: 2
Explanation:The longest repeating substrings are "ab" and "ba", each of which occurs twice.

```

**Example 3:**

```
Input: s = "aabcaabdaab"
Output: 3
Explanation:The longest repeating substring is "aab", which occurs3 times.

```

**Constraints:**

- `1 <= s.length <= 2000`
- `s` consists of lowercase English letters.

```cpp
class Solution {
public:
    int longestRepeatingSubstring(string s) {
        int n=s.size();
        unordered_map<string,int> mp;
        for(int i=0;i<n;i++){
            for(int j=i;j<n;j++){
                mp[s.substr(i,j-i+1)]++;
            }
        }
        int maxLen=0;
        for(auto& [s,cnt]: mp){
            if(cnt>1){
                maxLen=max(maxLen, (int)s.size());
            }
        }
        return maxLen;
    }
};

/*
brute force
    find all substr:
        for l, for r O(n^2)
            check if exist twice: mp[str,cnt]

for each possible substr starting idx
    len(idx~n-1) cut into half
        all possible substr is in the left half, otherwise it is too long

*/
```