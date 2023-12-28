# Largest Palindromic Number

#: 2384
Difficult: Medium
Tags: Greedy, Hash, string
link: https://leetcode.com/problems/largest-palindromic-number/description/
Priority: Low
Created time: December 15, 2023 10:34 PM
Last edited time: December 15, 2023 10:34 PM
practicedTimes: 0
source: microsoftFreq

You are given a string `num` consisting of digits only.

Return *the **largest palindromic** integer (in the form of a string) that can be formed using digits taken from* `num`. It should not contain **leading zeroes**.

**Notes:**

- You do **not** need to use all the digits of `num`, but you must use **at least** one digit.
- The digits can be reordered.

**Example 1:**

```
Input: num = "444947137"
Output: "7449447"
Explanation:
Use the digits "4449477" from "444947137" to form the palindromic integer "7449447".
It can be shown that "7449447" is the largest palindromic integer that can be formed.

```

**Example 2:**

```
Input: num = "00009"
Output: "9"
Explanation:
It can be shown that "9" is the largest palindromic integer that can be formed.
Note that the integer returned should not contain leading zeroes.

```

**Constraints:**

- `1 <= num.length <= 105`
- `num` consists of digits.

```cpp
class Solution {
public:
    string largestPalindromic(string num) {
        vector<int> freq(10,0);//0~9
        for(auto c: num){
            freq[c-'0']++;
        }
        string mid="";
        string left="";
        for(int x=9;x>=0;x--){
            int cnt=freq[x];
            if(cnt%2==1 && mid=="") mid=string(1,'0'+x);
            left += string(cnt/2,'0'+x);
        }
        if(left.front()=='0') left="";
        string right=left;
        reverse(right.begin(), right.end());
        string res=left+mid+right;
        return res==""?"0":res;
    }
};
```