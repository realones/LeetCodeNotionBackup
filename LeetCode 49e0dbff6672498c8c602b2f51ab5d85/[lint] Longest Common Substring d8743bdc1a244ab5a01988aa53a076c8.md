# [lint] Longest Common Substring

#: 79
Difficult: Medium
Tags: DP, StringMatchToMatrix
link: https://www.lintcode.com/problem/longest-common-substring
Priority: Medium
Created time: September 15, 2022 8:10 PM
Last edited time: October 29, 2023 10:32 PM
practicedTimes: 1
source: jiuzhang, 算高DP下

Given two strings, find the longest common substring.

Return the length of it.

Example:

```
Example 1:
	Input:  "ABCD" and "CBCE"
	Output:  2

	Explanation:
	Longest common substring is "BC"

Example 2:
	Input: "ABCD" and "EACB"
	Output:  1

	Explanation:
	Longest common substring is 'A' or 'C' or 'B'

```

Challenge
O(n x m) time and memory.

思路：
two str match，dup calculation exists -> DP w/ matrix 代表前K个字母组成的substr
本质上是两个loop，找从S[i] and T[j]开始，可以有多少连续chars相同。
所以在matrix里，呈现出来的一定是一条条从1开始数字递增的左上到右下45度角的斜线 (when S[i]==T[j])
e.g. 观察斜线：(when S[i]==T[j])

| 0 | 0 | 0 | 0 | 0 | 0 |
| --- | --- | --- | --- | --- | --- |
| 0 | 0 | 1 | 0 | 1 | 0 |
| 0 | 0 | 0 | 2 | 0 | 0 |
| 0 | 0 | 1 | 0 | 3 | 0 |
| 0 | 0 | 0 | 2 | 0 | 0 |
| 0 | 0 | 0 | 0 | 3 | 0 |

f: tmp result that A[i-1]==B[j-1]

```cpp
class Solution {
public:
    /**
     * @param A: A string
     * @param B: A string
     * @return: the length of the longest common substring.
     */
    int longestCommonSubstring(string &A, string &B) {
        // write your code here
        int m=A.size(); if(m==0){return 0;}
        int n=B.size(); if(n==0){return 0;}
        int res=0;
        vector<vector<int>> f(m+1, vector<int>(n+1, 0));
        for(int i=1; i<m+1; i++){
            for(int j=1; j<n+1; j++){
                if(A[i-1]==B[j-1]){
                    f[i][j] = f[i-1][j-1] + 1;
                }
                else{
                    f[i][j] = 0;
                }
                res=max(res,f[i][j]);
            }
        }
        return res;
    }
};
```