# Minimum ASCII Delete Sum for Two Strings

#: 712
Difficult: Medium
Tags: dp-TwoSeq
link: https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings/
Priority: Low
Created time: November 21, 2022 12:20 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, leetcode_daily, wp动归套路
related: Delete Operation for Two Strings (Delete%20Operation%20for%20Two%20Strings%207c71f3a27e594920b0e09f631632797d.md)

Given two strings `s1` and `s2`, return *the lowest **ASCII** sum of deleted characters to make two strings equal*.

**Example 1:**

```
Input: s1 = "sea", s2 = "eat"
Output: 231
Explanation: Deleting "s" from "sea" adds the ASCII value of "s" (115) to the sum.
Deleting "t" from "eat" adds 116 to the sum.
At the end, both strings are equal, and 115 + 116 = 231 is the minimum sum possible to achieve this.

```

**Example 2:**

```
Input: s1 = "delete", s2 = "leet"
Output: 403
Explanation: Deleting "dee" from "delete" to turn the string into "let",
adds 100[d] + 101[e] + 101[e] to the sum.
Deleting "e" from "leet" adds 101[e] to the sum.
At the end, both strings are equal to "let", and the answer is 100+101+101+101 = 403.
If instead we turned both strings into "lee" or "eet", we would get answers of 433 or 417, which are higher.

```

**Constraints:**

- `1 <= s1.length, s2.length <= 1000`
- `s1` and `s2` consist of lowercase English letters.

Solution1:

```jsx
class Solution {
public:
    int minimumDeleteSum(string s1, string s2) {
        int m=s1.size();
        int n=s2.size();
        vector<vector<int>> dp(m+1,vector<int>(n+1,0));
        
        for(int i=1; i<m+1; i++){dp[i][0]=dp[i-1][0]+s1[i-1];}
        for(int j=1; j<n+1; j++){dp[0][j]=dp[0][j-1]+s2[j-1];}
        
        for(int i=1;i<m+1;i++){
            for(int j=1;j<n+1;j++){
                if(s1[i-1]==s2[j-1]){
                    dp[i][j]=dp[i-1][j-1];
                }
                else{
                    dp[i][j]=min(dp[i-1][j]+s1[i-1],dp[i][j-1]+s2[j-1]);
                }
            }
        }
        return dp.back().back();
    }
};
```

Solution2:

```cpp
class Solution {
public:
    int minimumDeleteSum(string s1, string s2) {
        int lcs=LCS(s1,s2);
        int res=0;
        for(auto c: s1){res+=c;} res-=lcs;
        for(auto c: s2){res+=c;} res-=lcs;
        return res;
    }
    
    int LCS(string& word1, string& word2){
        int m=word1.size(), n=word2.size();
        //largest common subseq askII sum
        vector<vector<int>> f(m+1,vector<int>(n+1,0));
        
        for(int i=1;i<m+1;i++){
            for(int j=1;j<n+1;j++){
                if(word1[i-1]==word2[j-1]){
                    f[i][j]=f[i-1][j-1]+word1[i-1];
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