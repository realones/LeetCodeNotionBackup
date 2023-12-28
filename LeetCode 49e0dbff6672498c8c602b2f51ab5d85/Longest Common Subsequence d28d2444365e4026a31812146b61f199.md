# Longest Common Subsequence

#: 1143
Difficult: Medium
Tags: StringMatchToMatrix, dp-TwoSeq
link: https://leetcode.com/problems/longest-common-subsequence/
Priority: Medium
Created time: September 15, 2022 8:03 PM
Last edited time: November 12, 2023 10:40 PM
practicedTimes: 1
source: AmazonFreq, LC_75, jiuzhang, wp动归套路, 算高DP下

Given two strings `text1` and `text2`, return *the length of their longest **common subsequence**.* If there is no **common subsequence**, return `0`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

- For example, `"ace"` is a subsequence of `"abcde"`.

A **common subsequence** of two strings is a subsequence that is common to both strings.

**Example 1:**

```
Input: text1 = "abcde", text2 = "ace"
Output: 3
Explanation: The longest common subsequence is "ace" and its length is 3.

```

**Example 2:**

```
Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: The longest common subsequence is "abc" and its length is 3.

```

**Example 3:**

```
Input: text1 = "abc", text2 = "def"
Output: 0
Explanation: There is no such common subsequence, so the result is 0.

```

**Constraints:**

- `1 <= text1.length, text2.length <= 1000`
- `text1` and `text2` consist of only lowercase English characters.

要先看longest-common-substring，与其类似。
但是when S[i]==T[j]， 呈现出来的不是左上到右下45度角的递增斜线，而是左上到右下的一个个递增的点
e.g. 观察左上到右下的递增 (when S[i]==T[j])

![27BA5912-7A34-4E07-AA8C-B0C865F8F03D.png](Longest%20Common%20Subsequence%20d28d2444365e4026a31812146b61f199/27BA5912-7A34-4E07-AA8C-B0C865F8F03D.png)

```cpp
class Solution {
public:
    int longestCommonSubsequence(string s1, string s2) {
        int m=s1.size(), n=s2.size();
        vector<vector<int>> f(m+1,vector<int>(n+1,0));
        for(int i=1;i<m+1;i++){
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