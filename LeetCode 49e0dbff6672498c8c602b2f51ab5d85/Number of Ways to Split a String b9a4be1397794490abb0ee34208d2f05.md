# Number of Ways to Split a String

#: 1573
Difficult: Medium
Tags: Math, string
link: https://leetcode.com/problems/number-of-ways-to-split-a-string/description/
Priority: Medium
Status: toSummarize
Created time: June 27, 2023 11:37 PM
Last edited time: December 6, 2023 4:08 PM
source: HuaHua, ticktokFreq

Given a binary string `s`, you can split `s` into 3 **non-empty** strings `s1`, `s2`, and `s3` where `s1 + s2 + s3 = s`.

Return the number of ways `s` can be split such that the number of ones is the same in `s1`, `s2`, and `s3`. Since the answer may be too large, return it **modulo** `109 + 7`.

**Example 1:**

```
Input: s = "10101"
Output: 4
Explanation: There are four ways to split s in 3 parts where each part contain the same number of letters '1'.
"1|010|1"
"1|01|01"
"10|10|1"
"10|1|01"

```

**Example 2:**

```
Input: s = "1001"
Output: 0

```

**Example 3:**

```
Input: s = "0000"
Output: 3
Explanation: There are three ways to split s in 3 parts.
"0|0|00"
"0|00|0"
"00|0|0"

```

**Constraints:**

- `3 <= s.length <= 105`
- `s[i]` is either `'0'` or `'1'`.

Solution:

toSummarize: the way to find out way1 and way2 is amazing

```cpp
//from ans to save time
//https://leetcode.com/problems/number-of-ways-to-split-a-string/solutions/905967/c-easiest-way-runtime-beats-99-52/
class Solution {
public:
    int numWays(string s) {
        long mod=1000000007;
        long ones=0;
        long n=s.size();
		//Count number of Ones
        for(int i=0;i<n;i++) 
            ones+=s[i]-'0';
        if(ones==0)  //When there are only zeroes
		       return (int)((((n-1)*(n-2))/2)%mod);
        if(ones%3!=0)  //Can't be grouped equally if the ones are not divisible by 3
		       return 0;
        long onethird=ones/3;  //Number of ones in each group
        ones=0;
		//Number of zeroes which lie on the borders
        long way1=0,way2=0;
        for(int i=0;i<n;i++){
            ones+=s[i]-'0';
            if(ones==onethird) way1++;
            if(ones==2*onethird) way2++;
        }
        return (int)((way1*way2)%mod);
    }
};

/*
s:
    len: 3~1e5
    val: 0 or 1
================================
if %3!=0 return 0
each substr contains 1s of cnt1s / 3

....1XXX1....1XXXX1....
     len1     len2
possiblities:
    for len1: len1+1
    *
    for len2: len2+1
*/
```