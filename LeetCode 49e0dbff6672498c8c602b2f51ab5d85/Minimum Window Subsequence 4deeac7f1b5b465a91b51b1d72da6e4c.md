# Minimum Window Subsequence

#: 727
Difficult: Hard
Tags: dp-TwoSeq
link: https://leetcode.com/problems/minimum-window-subsequence/
Priority: High
Status: Dig More
Created time: November 20, 2022 11:27 PM
Last edited time: October 12, 2023 2:56 PM
source: wp动归套路

Given strings `s1` and `s2`, return *the minimum contiguous substring part of* `s1`*, so that* `s2` *is a subsequence of the part*.

If there is no such window in `s1` that covers all characters in `s2`, return the empty string `""`. If there are multiple such minimum-length windows, return the one with the **left-most starting index**.

**Example 1:**

```
Input: s1 = "abcdebdde", s2 = "bde"
Output: "bcde"
Explanation:
"bcde" is the answer because it occurs before "bdde" which has the same length.
"deb" is not a smaller window because the elements of s2 in the window must occur in order.

```

**Example 2:**

```
Input: s1 = "jmeqksfrsdcmsiwvaovztaqenprpvnbstl", s2 = "u"
Output: ""

```

**Constraints:**

- `1 <= s1.length <= 2 * 104`
- `1 <= s2.length <= 100`
- `s1` and `s2` consist of lowercase English letters.

```cpp
class Solution {
public:
    string minWindow(string s1, string s2) {
        int m=s1.size(), n=s2.size();
        vector<vector<int>> dp(m+1,vector<int>(n+1,INT_MAX));
        for(int i=0; i<m+1; i++){dp[i][0]=0;}
        
        for(int i=1;i<m+1;i++){
            for(int j=1;j<n+1;j++){
                if(s1[i-1]==s2[j-1]){
                    if(dp[i-1][j-1]==INT_MAX) continue;
                    dp[i][j]=dp[i-1][j-1]+1;
                }
                else{
                    if(dp[i-1][j]==INT_MAX) continue;
                    dp[i][j]=dp[i-1][j]+1;
                }
            }
        }
        
        string res="";
        int minLen=INT_MAX;
        for(int i=1; i<m+1; i++){
            if(dp[i].back()<minLen){
                minLen=dp[i].back();
                res=s1.substr(i-minLen,minLen);
            }
        }
        return res;
    }
};
```