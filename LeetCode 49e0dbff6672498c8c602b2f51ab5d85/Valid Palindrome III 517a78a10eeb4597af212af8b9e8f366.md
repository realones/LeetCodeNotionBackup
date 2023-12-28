# Valid Palindrome III

#: 1216
Difficult: Hard
Tags: dp-TwoSeq
link: https://leetcode.com/problems/valid-palindrome-iii/
Priority: Low
Status: dup
Created time: November 21, 2022 10:53 AM
Last edited time: October 28, 2023 12:48 AM
source: wp动归套路
related: Longest Palindromic Substring (Longest%20Palindromic%20Substring%20ea1a3cc3811747edba0fc9d58fe1d494.md), Longest Palindromic Subsequence (Longest%20Palindromic%20Subsequence%2084aa3d867a9e4aca9510bdf16a0cbf4c.md), Minimum Number of Groups to Create a Valid Assignment (Minimum%20Number%20of%20Groups%20to%20Create%20a%20Valid%20Assignm%202ffa5feedc98428d97bda3bf5b4eca00.md)

Given a string `s` and an integer `k`, return `true` if `s` is a `k`**-palindrome**.

A string is `k`**-palindrome** if it can be transformed into a palindrome by removing at most `k` characters from it.

**Example 1:**

```
Input: s = "abcdeca", k = 2
Output: true
Explanation: Remove 'b' and 'e' characters.

```

**Example 2:**

```
Input: s = "abbababa", k = 1
Output: true

```

**Constraints:**

- `1 <= s.length <= 1000`
- `s` consists of only lowercase English letters.
- `1 <= k <= s.length`

```cpp
class Solution {
public:
    bool isValidPalindrome(string s, int k) {
        string rs=s;
        reverse(rs.begin(),rs.end());
        int lcs=LCS(s,rs);
        return s.size()-lcs <= k;
    }
    
    int LCS(string& s1, string& s2){
        int n=s1.size();
        vector<vector<int>> f(n+1,vector<int>(n+1,0));
        
        for(int i=1;i<n+1;i++){
            for(int j=1;j<n+1;j++){
                if(s1[i-1]==s2[j-1]){
                    f[i][j]=f[i-1][j-1]+1;
                }
                else{
                    f[i][j]=max(f[i][j-1], f[i-1][j]);
                }
            }
        }
        
        return f.back().back();
    }
};
```