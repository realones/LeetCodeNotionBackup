# Longest Palindromic Subsequence

#: 516
Difficult: Medium
Tags: dp-Pyr
link: https://leetcode.com/problems/longest-palindromic-subsequence/
Priority: Medium
Created time: December 12, 2022 11:15 PM
Last edited time: October 28, 2023 12:40 AM
source: HuaHua, wp动归套路
related: Longest Palindromic Substring (Longest%20Palindromic%20Substring%20ea1a3cc3811747edba0fc9d58fe1d494.md), Valid Palindrome III (Valid%20Palindrome%20III%20517a78a10eeb4597af212af8b9e8f366.md)

Given a string `s`, find *the longest palindromic **subsequence**'s length in* `s`.

A **subsequence** is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

```
Input: s = "bbbab"
Output: 4
Explanation: One possible longest palindromic subsequence is "bbbb".

```

**Example 2:**

```
Input: s = "cbbd"
Output: 2
Explanation: One possible longest palindromic subsequence is "bb".

```

**Constraints:**

- `1 <= s.length <= 1000`
- `s` consists only of lowercase English letters.

```jsx
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int n=s.size();
        
        vector<vector<int>> f(n,vector<int>(n,0));//init as 0 for not exist ones
        for(int i=0; i<n; i++){f[i][i]=1;}//init len=1
        for(int len=2;len<=n;len++){
            for(int l=0,r;r=l+len-1,r<n;l++){
                if(s[l]==s[r]){
                    f[l][r]=f[l+1][r-1]+2;
                }
                else{
                    f[l][r]=max(f[l+1][r], f[l][r-1]);
                }
            }
        }
        return f[0][n-1];
    }
};

/*
pyr:
dp(ij) 
    i==j 1
    else
        if [i]!=[j] max(i,j-1), (i+1,j)
return dp0n-1
*/
```