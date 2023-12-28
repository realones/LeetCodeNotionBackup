# Shortest Way to Form String

#: 1055
Difficult: Medium
Tags: 2pt_SameDir_diffSpeed, Greedy
link: https://leetcode.com/problems/shortest-way-to-form-string/
Priority: Medium
Created time: October 24, 2022 1:06 AM
Last edited time: October 12, 2023 2:56 PM
source: googleFreq

A **subsequence** of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).

Given two strings `source` and `target`, return *the minimum number of **subsequences** of* `source` *such that their concatenation equals* `target`. If the task is impossible, return `-1`.

**Example 1:**

```
Input: source = "abc", target = "abcbc"
Output: 2
Explanation: The target "abcbc" can be formed by "abc" and "bc", which are subsequences of source "abc".

```

**Example 2:**

```
Input: source = "abc", target = "acdbc"
Output: -1
Explanation: The target string cannot be constructed from the subsequences of source string due to the character "d" in target string.

```

**Example 3:**

```
Input: source = "xyz", target = "xzyxz"
Output: 3
Explanation: The target string can be constructed as follows "xz" + "y" + "xz".

```

**Constraints:**

- `1 <= source.length, target.length <= 1000`
- `source` and `target` consist of lowercase English letters.

```cpp
/*
ti -->
si --> iterator again and again, end link to start, use offset
    everytime s[0] is scanned, res++
*/
class Solution {
public:
    int shortestWay(string source, string target) {
        unordered_set<char> st(source.begin(), source.end());
        for(auto c:target){
            if(!st.count(c)) return -1;
        }
        
        int sSz=source.size(), tSz=target.size();
        int res=0;
        for(int ti=0,si=0; ti<tSz; ti++){
            while(source[si]!=target[ti]){
                if(si==0) res++;
                si=(si+1)%sSz;
            }
            if(si==0) res++;
            si=(si+1)%sSz;
        }
        return res;
    }
};
```