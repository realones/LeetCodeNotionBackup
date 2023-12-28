# Minimum Insertion Steps to Make a String Palindrome

#: 1312
Difficult: Hard
Tags: dp-TwoSeq
link: https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/
Priority: Low
Status: dup
Created time: November 21, 2022 11:25 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, wp动归套路
related: Longest Palindromic Substring (Longest%20Palindromic%20Substring%20ea1a3cc3811747edba0fc9d58fe1d494.md)

Given a string `s`. In one step you can insert any character at any index of the string.

Return *the minimum number of steps* to make `s` palindrome.

A **Palindrome String** is one that reads the same backward as well as forward.

**Example 1:**

```
Input: s = "zzazz"
Output: 0
Explanation: The string "zzazz" is already palindrome we do not need any insertions.

```

**Example 2:**

```
Input: s = "mbadm"
Output: 2
Explanation: String can be "mbdadbm" or "mdbabdm".

```

**Example 3:**

```
Input: s = "leetcode"
Output: 5
Explanation: Inserting 5 characters the string becomes "leetcodocteel".

```

**Constraints:**

- `1 <= s.length <= 500`
- `s` consists of lowercase English letters.

```cpp
class Solution {
public:
    int minInsertions(string s) {
        string rs=s;
        reverse(rs.begin(),rs.end());
        int lcs=LCS(s,rs);
        return s.size()-lcs;
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