# Remove All Adjacent Duplicates In String

#: 1047
Difficult: Easy
Tags: stack, string
link: https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/description/
Priority: Pass
Created time: December 3, 2023 3:46 PM
Last edited time: December 3, 2023 3:46 PM
source: facebookFreq

You are given a string `s` consisting of lowercase English letters. A **duplicate removal** consists of choosing two **adjacent** and **equal** letters and removing them.

We repeatedly make **duplicate removals** on `s` until we no longer can.

Return *the final string after all such duplicate removals have been made*. It can be proven that the answer is **unique**.

**Example 1:**

```
Input: s = "abbaca"
Output: "ca"
Explanation:
For example, in "abbaca" we could remove "bb" since the letters are adjacent and equal, and this is the only possible move.  The result of this move is that the string is "aaca", of which only "aa" is possible, so the final string is "ca".

```

**Example 2:**

```
Input: s = "azxxzy"
Output: "ay"

```

**Constraints:**

- `1 <= s.length <= 105`
- `s` consists of lowercase English letters.

```cpp
class Solution {
public:
    string removeDuplicates(string s) {
        stack<char> stk;
        for(auto c: s){
            if(stk.empty()==false && stk.top()==c) stk.pop();
            else stk.push(c);
        }
        string res;
        while(!stk.empty()){
            res+=stk.top(); stk.pop();
        }
        reverse(res.begin(), res.end());
        return res;
    }
};

/*
s:
   len: 1~1e5
   val: lower case: 26
================================

*/
```