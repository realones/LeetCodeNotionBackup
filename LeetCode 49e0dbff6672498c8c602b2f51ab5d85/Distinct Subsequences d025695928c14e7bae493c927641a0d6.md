# Distinct Subsequences

#: 115
Difficult: Hard
Tags: DP, StringMatchToMatrix, dp-TwoSeq
link: https://leetcode.com/problems/distinct-subsequences/
Priority: Medium
Created time: September 15, 2022 8:57 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, jiuzhang, wp动归套路, 算高DP下

Given two strings `s` and `t`, return *the number of distinct subsequences of `s` which equals `t`*.

A string's **subsequence** is a new string formed from the original string by deleting some (can be none) of the characters without disturbing the remaining characters' relative positions. (i.e., `"ACE"` is a subsequence of `"ABCDE"` while `"AEC"` is not).

The test cases are generated so that the answer fits on a 32-bit signed integer.

**Example 1:**

```
Input: s = "rabbbit", t = "rabbit"
Output: 3
Explanation:
As shown below, there are 3 ways you can generate "rabbit" from s.
rabbbitrabbbitrabbbit
```

**Example 2:**

```
Input: s = "babgbag", t = "bag"
Output: 5
Explanation:
As shown below, there are 5 ways you can generate "bag" from s.
babgbagbabgbagbabgbagbabgbagbabgbag
```

**Constraints:**

- `1 <= s.length, t.length <= 1000`
- `s` and `t` consist of English letters.

Solution 1:

```cpp
class Solution {
public:
    int numDistinct(string s, string t) {
        int m=s.size(), n=t.size();
        
        vector<vector<long>> f(m+1,vector<long>(n+1,0));
        for(int i=0; i<m+1; i++){f[i][0]=1;}
        
        for(int i=1;i<m+1;i++){
            for(int j=1;j<n+1;j++){
                if(s[i-1]==t[j-1]){
                    f[i][j]=f[i-1][j]+f[i-1][j-1];
                }
                else{
                    f[i][j]=f[i-1][j];
                }
            }
        }
        
        return f.back().back();
    }
};
```

Solution 2:

我的思路：

要先看longest-common-substring，与其类似。

但是target str的每一个char都会依次被对应到，所以要找一共有多少从row1一直到row(n-1)的斜线

e.g. 观察左上到右下的斜线（横source，纵target）

（*）means match

|  | *1 | *2 | *3 |  |  |  |  |  |  | *4 |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| *0 |  |  |  | *3 |  |  |  | *4 |  |  |  |  |  |
|  |  |  |  |  |  | * |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  | * |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  | * |  |  |  |  |
|  |  |  |  |  | * |  |  |  |  |  | * |  |  |
|  |  |  |  |  |  |  |  |  |  |  |  | * |  |

if not match, val same as left one
if match, val = left + left_up

```cpp
class Solution {
public:
    int numDistinct(string s, string t) {
        //s - sub
        //t - whole
        //equal
        //subsequence but t is whole
        //so in matrix
        //from 0 to n-1, each char in t should be mapped
        //then line from left up to bottom right, cnt how many possible lines
        //
        //f(i,j): i,j are lens of substr of s and t
        //        s.substr(0,i) to t.substr(0,j), tmp res
        //f(i,j) = 
        //      if match, val = left + left_up 
        //      it not  , val same as left
        //res: f(m,n)
        //init: f(0,x) = 1
        //      f(x,0) = 0
        int m=t.size();
        int n=s.size();
        vector<vector<long>> matrix(m+1, vector<long>(n+1, 0));
        for(int j=0;j<n+1;j++){
            matrix[0][j]=1;
        }
        for(int i=1; i<m+1; i++){
            for(int j=1; j<n+1; j++){
                if(t[i-1]==s[j-1]){
                    matrix[i][j]=matrix[i][j-1]+matrix[i-1][j-1];
                }
                else{
                    matrix[i][j]=matrix[i][j-1];
                }
            }
        }
        return matrix[m][n];
    }
};
```