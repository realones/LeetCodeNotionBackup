# Interleaving String

#: 97
Difficult: Medium
Tags: DP, StringMatchToMatrix, TopSorting, dp-TwoSeq
link: https://leetcode.com/problems/interleaving-string/
Priority: Medium
Status: More Solution
Created time: September 15, 2022 9:25 PM
Last edited time: October 29, 2023 10:48 PM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, jiuzhang, wp动归套路, 算高DP下

Given strings `s1`, `s2`, and `s3`, find whether `s3` is formed by an **interleaving** of `s1` and `s2`.

An **interleaving** of two strings `s` and `t` is a configuration where `s` and `t` are divided into `n` and `m` **non-empty** substrings respectively, such that:

- `s = s1 + s2 + ... + sn`
- `t = t1 + t2 + ... + tm`
- `|n - m| <= 1`
- The **interleaving** is `s1 + t1 + s2 + t2 + s3 + t3 + ...` or `t1 + s1 + t2 + s2 + t3 + s3 + ...`

**Note:** `a + b` is the concatenation of strings `a` and `b`.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/09/02/interleave.jpg](https://assets.leetcode.com/uploads/2020/09/02/interleave.jpg)

```
Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
Output: true
Explanation: One way to obtain s3 is:
Split s1 into s1 = "aa" + "bc" + "c", and s2 into s2 = "dbbc" + "a".
Interleaving the two splits, we get "aa" + "dbbc" + "bc" + "a" + "c" = "aadbbcbcac".
Since s3 can be obtained by interleaving s1 and s2, we return true.

```

**Example 2:**

```
Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
Output: false
Explanation: Notice how it is impossible to interleave s2 with any other string to obtain s3.

```

**Example 3:**

```
Input: s1 = "", s2 = "", s3 = ""
Output: true

```

**Constraints:**

- `0 <= s1.length, s2.length <= 100`
- `0 <= s3.length <= 200`
- `s1`, `s2`, and `s3` consist of lowercase English letters.

**Follow up:** Could you solve it using only `O(s2.length)` additional memory space?

todo: possible solution? 

maintain ind and outd for each char, check if s3 is a correct iteration solution.

interleaving(s1, s2) ?= s3
<-
interleaving(s1-last , s2       ) ?= s3-last && s1.last=s3.last
interleaving(s1        , s2-last) ?= s3-last && s2.last=s3.last
同时发现有重复计算，then DP

```cpp
class Solution {
public:
    /**
     * @param s1: A string
     * @param s2: A string
     * @param s3: A string
     * @return: Determine whether s3 is formed by interleaving of s1 and s2
     */
     
    /*
    匹配类 dp
    f(i,j) 表示s1的前i个字符和s2的前j个字符能否交替组成s3的前i+j个字符
    
    f(i,j)=
    1. f(i-1,j) && s1[i-1]==s3[i+j-1]
    ||
    2. f(i,j-1) && s2[j-1]==s3[i+j-1]
    
    init: 
    f(0,0)=true, 
    f(i,0)= s1.substr(0,i)==s3.substr(0,i)
    f(0,j)= s2.substr(0,j)==s3.substr(0,j)
    
    return: f(m,n)
    */ 
    
    bool isInterleave(string &s1, string &s2, string &s3) {
        int m=s1.size(); if(m==0) return s2==s3;
        int n=s2.size(); if(n==0) return s1==s3;
        if((m+n)!=s3.size()) return false;
        
        vector<vector<bool>> dp(m+1,vector<bool>(n+1,false));
        //init
        dp[0][0]=true;
        for(int i=1;i<m+1;i++){dp[i][0]= s1.substr(0,i)==s3.substr(0,i); }
        for(int j=1;j<n+1;j++){dp[0][j]= s2.substr(0,j)==s3.substr(0,j); }
        //process
        for(int i=1;i<m+1;i++){
            for(int j=1;j<n+1;j++){
                dp[i][j]=dp[i-1][j]&&(s1[i-1]==s3[i+j-1]) || dp[i][j-1]&&(s2[j-1]==s3[i+j-1]);
            }
        }
        //return
        return dp[m][n];
    }
};
```