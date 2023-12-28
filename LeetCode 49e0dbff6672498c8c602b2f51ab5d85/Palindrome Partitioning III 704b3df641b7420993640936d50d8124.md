# Palindrome Partitioning III

#: 1278
Difficult: Hard
Tags: dp-K_Ranges, string
link: https://leetcode.com/problems/palindrome-partitioning-iii/
Priority: High
Status: Dig More
Created time: December 1, 2022 3:54 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, wp动归套路

You are given a string `s` containing lowercase letters and an integer `k`. You need to :

- First, change some characters of `s` to other lowercase English letters.
- Then divide `s` into `k` non-empty disjoint substrings such that each substring is a palindrome.

Return *the minimal number of characters that you need to change to divide the string*.

**Example 1:**

```
Input: s = "abc", k = 2
Output: 1
Explanation: You can split the string into "ab" and "c", and change 1 character in "ab" to make it palindrome.

```

**Example 2:**

```
Input: s = "aabbc", k = 3
Output: 0
Explanation: You can split the string into "aa", "bb" and "c", all of them are palindrome.
```

**Example 3:**

```
Input: s = "leetcode", k = 8
Output: 0

```

**Constraints:**

- `1 <= k <= s.length <= 100`.
- `s` only contains lowercase English letters.

```cpp
class Solution {
public:
    int palindromePartition(string s, int K) {
        int n=s.size();
        //get cost[l,r]
        vector<vector<int>> cost(n,vector<int>(n,0));
        for(int len=2;len<=n;len++){
            for(int l=0,r; r=l+len-1,r<n; l++){
                cost[l][r]= cost[l+1][r-1] + (s[l]!=s[r]);
            }
        }
        //prcoess
        vector<vector<int>> dp(n,vector<int>(K+1,INT_MAX/2));
        for(int i=0;i<n;i++){
            for(int k=1;k<=min(K,i+1);k++){
                //for(int j=k-1; j<=i; j++){
                for(int j=i; j>=k-1; j--){
                    dp[i][k]=min(dp[i][k], (j-1>=0?dp[j-1][k-1]:0) + cost[j][i]);
                }
            }
        }
				return dp.back().back()
    }
};

/*
s -> divide to K pieces
dp[idx: 0~n-1][k pieces: 0~K]: min char changes

for i: 0~n-1
    for k: 1~min(K,i+1)
        dp[i][k] min= dp[j-1][k-1] + costji, for j: k-1~i

costji= changes need to for s.substr(j,i-j+1);

init: all invalid: INT_MAX 

return dp.bb
*/
```