# Decoded String at Index

#: 880
Difficult: Medium
Tags: recursion, stack, string
link: https://leetcode.com/problems/decoded-string-at-index/
Priority: High
Status: Dig More, More Solution, No Idea At First, toRevisit
Created time: October 3, 2023 10:53 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

You are given an encoded string `s`. To decode the string to a tape, the encoded string is read one character at a time and the following steps are taken:

- If the character read is a letter, that letter is written onto the tape.
- If the character read is a digit `d`, the entire current tape is repeatedly written `d - 1` more times in total.

Given an integer `k`, return *the* `kth` *letter (**1-indexed)** in the decoded string*.

**Example 1:**

```
Input: s = "leet2code3", k = 10
Output: "o"
Explanation: The decoded string is "leetleetcodeleetleetcodeleetleetcode".
The 10th letter in the string is "o".

```

**Example 2:**

```
Input: s = "ha22", k = 5
Output: "h"
Explanation: The decoded string is "hahahaha".
The 5th letter is "h".

```

**Example 3:**

```
Input: s = "a2345678999999999999999", k = 1
Output: "a"
Explanation: The decoded string is "a" repeated 8301530446056247680 times.
The 1st letter is "a".

```

**Constraints:**

- `2 <= s.length <= 100`
- `s` consists of lowercase English letters and digits `2` through `9`.
- `s` starts with a letter.
- `1 <= k <= 109`
- It is guaranteed that `k` is less than or equal to the length of the decoded string.
- The decoded string is guaranteed to have less than `263` letters.

Solution wisdompeak:

```cpp
class Solution {
public:
    string decodeAtIndex(string s, int k) {
        int n=s.size();
        long long cnt=0;
        for(int i=0;i<n;i++){
            char c=s[i];
            if(isalpha(c)){
                cnt++;
                if(cnt==k) return string(1,c);
            }
            else{//digit
                long long d=c-'0';
                if(k<=cnt*d) return decodeAtIndex(s.substr(0,i),(k%cnt)!=0?k%cnt:cnt);
                else cnt*=d;
            }
        }
        return "";
    }
};

/*
f(s,k) = 
    maintain cnt(init 0), which is the cnt_th char, in the end, cnt==k
    for c 
        if c, cnt++ for each char, cnt==k, then return c
        if d, then 
            if k <  cnt*d, then f(cur-1, k%cnt)
            if k == cnt*d, then return s.(cur-1), which is f(cur-1, cnt)
            if k >  cnt*d, cnt=cnt*d
*/
```