# Shortest Common Supersequence

#: 1092
Difficult: Hard
Tags: dp-TwoSeq
link: https://leetcode.com/problems/shortest-common-supersequence/
Priority: Low
Status: More Solution
Created time: November 20, 2022 7:29 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, wp动归套路

Given two strings `str1` and `str2`, return *the shortest string that has both* `str1` *and* `str2` *as **subsequences***. If there are multiple valid strings, return **any** of them.

A string `s` is a **subsequence** of string `t` if deleting some number of characters from `t` (possibly `0`) results in the string `s`.

**Example 1:**

```
Input: str1 = "abac", str2 = "cab"
Output: "cabac"
Explanation:
str1 = "abac" is a subsequence of "cabac" because we can delete the first "c".
str2 = "cab" is a subsequence of "cabac" because we can delete the last "ac".
The answer provided is the shortest such string that satisfies these properties.

```

**Example 2:**

```
Input: str1 = "aaaaaaaa", str2 = "aaaaaaaa"
Output: "aaaaaaaa"

```

**Constraints:**

- `1 <= str1.length, str2.length <= 1000`
- `str1` and `str2` consist of lowercase English letters.

Solution: TLE

```cpp
class Solution {
public:
    string shortestCommonSupersequence(string str1, string str2) {
        int m=str1.size(); if(m==0) return str2;
        int n=str2.size(); if(n==0) return str1;
        
        vector<vector<string>> f(m+1,vector<string>(n+1,""));
        
        for(int i=1; i<=m; i++){
            f[i][0]=str1.substr(0,i);
        }
        for(int j=1; j<=n; j++){
            f[0][j]=str2.substr(0,j);
        }
        
        for(int i=1;i<m+1;i++){
            for(int j=1;j<n+1;j++){
                if(str1[i-1]==str2[j-1]){
                    f[i][j]=f[i-1][j-1]+str1[i-1];
                }
                else{
                    string s1=f[i][j-1]+str2[j-1];
                    string s2=f[i-1][j]+str1[i-1];
                    f[i][j]=s1.size()<s2.size()? s1: s2;
                }
            }
        }
        
        return f.back().back();
    }
};
```

Solution from discuss:

```cpp
string shortestCommonSupersequence(string& A, string& B) {
        int i = 0, j = 0;
        string res = "";
        for (char c : lcs(A, B)) {
            while (A[i] != c)
                res += A[i++];
            while (B[j] != c)
                res += B[j++];
            res += c, i++, j++;
        }
        return res + A.substr(i) + B.substr(j);
    }

    string lcs(string& A, string& B) {
        int n = A.size(), m = B.size();
        vector<vector<string>> dp(n + 1, vector<string>(m + 1, ""));
        for (int i = 0; i < n; ++i)
            for (int j = 0; j < m; ++j)
                if (A[i] == B[j])
                    dp[i + 1][j + 1] = dp[i][j] + A[i];
                else
                    dp[i + 1][j + 1] = dp[i + 1][j].size() > dp[i][j + 1].size() ?  dp[i + 1][j] : dp[i][j + 1];
        return dp[n][m];
    }
```