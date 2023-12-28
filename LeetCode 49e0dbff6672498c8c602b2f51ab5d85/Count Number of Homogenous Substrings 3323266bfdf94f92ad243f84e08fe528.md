# Count Number of Homogenous Substrings

#: 1759
Difficult: Medium
Tags: Math, string
link: https://leetcode.com/problems/count-number-of-homogenous-substrings
Priority: Low
Created time: November 11, 2023 2:12 AM
Last edited time: November 11, 2023 2:14 AM
source: leetcode_daily

Given a string `s`, return *the number of **homogenous** substrings of* `s`*.* Since the answer may be too large, return it **modulo** `109 + 7`.

A string is **homogenous** if all the characters of the string are the same.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

```
Input: s = "abbcccaa"
Output: 13
Explanation: The homogenous substrings are listed as below:
"a"   appears 3 times.
"aa"  appears 1 time.
"b"   appears 2 times.
"bb"  appears 1 time.
"c"   appears 3 times.
"cc"  appears 2 times.
"ccc" appears 1 time.
3 + 1 + 2 + 1 + 3 + 2 + 1 = 13.
```

**Example 2:**

```
Input: s = "xy"
Output: 2
Explanation: The homogenous substrings are "x" and "y".
```

**Example 3:**

```
Input: s = "zzzzz"
Output: 15

```

**Constraints:**

- `1 <= s.length <= 105`
- `s` consists of lowercase letters.

**Solution:**

```cpp
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int countHomogenous(string s) {
        ll res=0;
        ll len=0;
        char cur=s[0];
        s.push_back('#');
        //for each pieces with same c, res+=len*(len+1)/2
        for(auto c: s){
            if(c==cur){
                len++;
            }
            else{
                res+=len*(len+1)/2;
                res%=M;
                cur=c;
                len=1;
            }
        }
        return res;
    }
};

/*
1: 1
x: 1+2+
*
```