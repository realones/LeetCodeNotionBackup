# Longest Palindromic Substring

#: 5
Difficult: Medium
Tags: DP, Manacher, string
link: https://leetcode.com/problems/longest-palindromic-substring/
Priority: High
Status: Dig More, In Progress, No Idea At First
Created time: June 6, 2023 11:52 PM
Last edited time: November 17, 2023 1:31 AM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, ticktokFreq
related: Longest Palindromic Subsequence (Longest%20Palindromic%20Subsequence%2084aa3d867a9e4aca9510bdf16a0cbf4c.md), Valid Palindrome III (Valid%20Palindrome%20III%20517a78a10eeb4597af212af8b9e8f366.md), Minimum Insertion Steps to Make a String Palindrome (Minimum%20Insertion%20Steps%20to%20Make%20a%20String%20Palindrom%209bb48427b933450ca691a199ed559c53.md)

Given a string `s`, return *the longest* *palindromic* *substring* in `s`.

**Example 1:**

```
Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.

```

**Example 2:**

```
Input: s = "cbbd"
Output: "bb"

```

**Constraints:**

- `1 <= s.length <= 1000`
- `s` consist of only digits and English letters.

Solution 1: - complexity is worse than the best solution

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        int n=s.size();
        int maxLen=0;
        string res;
        //odd
        for(int i=0; i<n; i++){
            int l=i,r=i;
            while(l-1>=0 && r+1<n && s[l-1]==s[r+1]){
                l--,r++;
            }
            if(r-l+1 > maxLen){
                maxLen=r-l+1;
                res=s.substr(l,r-l+1);
            }
        }
        //even
        for(int i=0; i<n-1; i++){//i,i+1
            int l=i,r=i+1;
            if(s[l]!=s[r]) continue;
            while(l-1>=0 && r+1<n && s[l-1]==s[r+1]){
                l--,r++;
            }
            if(r-l+1 > maxLen){
                maxLen=r-l+1;
                res=s.substr(l,r-l+1);
            }
        }
        return res;
    }
};
```

Solution - WRONG - two seq - FAILED CASE: 

Input: "aacabdkacaa"
Output: "aaca"
Expected: "aca"

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        int n=s.size(); if(n==0) return "";
        string t=s;
        reverse(t.begin(),t.end());
        //find longest common substr
        int len=0, end=-1;
        vector<vector<int>> dp(n+1,vector<int>(n+1,0));
        for(int i=1;i<n+1;i++){
            for(int j=1;j<n+1;j++){
                if(s[i-1]==t[j-1]){
                    dp[i][j]=dp[i-1][j-1]+1;
                }
                else{
                    dp[i][j]=0;
                }
                if(dp[i][j]>len){
                    len=dp[i][j];
                    end=i-1;
                }
            }
        }
        return s.substr(end-len+1,len);
    }
};
```

Solution - old:

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        int center = 0, reach = 0, ansstart = 0, anslength = 0;
        
        string tmp = "!#";
        for(int i = 0; i < s.length(); i ++)
            tmp += s[i], tmp += '#';
        tmp + '!';
        
        vector<int> r(tmp.length());
        
        for(int i = 0; i < tmp.length(); i ++)
        {
            int mirrori = center * 2 - i;
            r[i] = reach > i ? min(r[mirrori], reach - i) : 0;
            for(; tmp[i + r[i] + 1] == tmp[i - r[i] - 1]; r[i] ++);
            if(i + r[i] > reach) reach = i + r[i], center = i;
            if(r[i] > anslength)
            {
                ansstart = i - r[i] >> 1;
                anslength = r[i];
            }
        }
        
        return s.substr(ansstart, anslength);
    }
};
```