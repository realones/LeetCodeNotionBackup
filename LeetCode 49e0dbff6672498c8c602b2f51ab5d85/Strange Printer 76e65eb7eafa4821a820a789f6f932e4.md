# Strange Printer

#: 664
Difficult: Hard
Tags: dp-Pyr, dp-状态转移不仅依赖前一项, string
link: https://leetcode.com/problems/strange-printer/description/
Priority: High
Status: Dig More, More Solution, No Idea At First, NotUnderstand, toSummarize
Created time: June 28, 2023 12:54 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, leetcode_daily

There is a strange printer with the following two special properties:

- The printer can only print a sequence of **the same character** each time.
- At each turn, the printer can print new characters starting from and ending at any place and will cover the original existing characters.

Given a string `s`, return *the minimum number of turns the printer needed to print it*.

**Example 1:**

```
Input: s = "aaabbb"
Output: 2
Explanation: Print "aaa" first and then print "bbb".

```

**Example 2:**

```
Input: s = "aba"
Output: 2
Explanation: Print "aaa" first and then print "b" from the second place of the string, which will cover the existing character 'a'.

```

**Constraints:**

- `1 <= s.length <= 100`
- `s` consists of lowercase English letters.

Solution 1:

[https://github.com/wisdompeak/LeetCode/tree/master/Dynamic_Programming/664.Strange-Printer](https://github.com/wisdompeak/LeetCode/tree/master/Dynamic_Programming/664.Strange-Printer)

================================

Intuitive:

dp-range:

*是否考虑区间外对区间内的影响

区间外：不考虑 - blindly

区间内：

打印长一点：把后面的重复字符也覆盖 → 对于区间[l,r], 都打印成s[l]

*影响：无法把区间进一步转换。

*思考：前面的x和后面的x是否要同一批次打印出来，不一定

================================

- option 1:
    - for range [l,r], operations
        1. print char c to [l,r], divide to pieces by char_c, handle each pieces
        2. divide to two pieces, handle [l,k] and [k+1,r]
- option 2:
    - for range [l,r], divide to
        - left - print with s[l]
        - right - not affected by outside

```cpp
class Solution {
public:
    int strangePrinter(string s) {
        int n=s.size();
        vector<vector<int>> dp(n,vector<int>(n,0));
        for(int i=0; i<n; i++){
            dp[i][i]=1;
        }
        for(int len=2;len<=n;len++){//len of a brick
            for(int l=0,r;r=l+len-1,r<n;l++){//for each brick
                dp[l][r]=1+dp[l+1][r];

                for(int k=l+1;k<=r;k++){
                    if(s[k]==s[l]){
                        dp[l][r] = min(dp[l][r],
                            dp[l][k-1] + ((k==r)?0:dp[k+1][r])
                        );
                    }
                }
            }
        }
        return dp[0][n-1];
    }
};

/*
n: 1~100
lower case eng char only
================================

dp[l][r] min= dp[l][k-1] + dp[k+1][r], for all l<=k<=r && s[l]==s[k]
=>
dp[l][r] min= 
    1 + dp[l+1][r]
    dp[l][k-1] + dp[k+1][r], for all l<k<=r
*
```