# Valid Parentheses

#: 20
Difficult: Easy
Tags: stack
link: https://leetcode.com/problems/valid-parentheses/
Priority: Pass
Created time: October 16, 2022 8:34 PM
Last edited time: November 3, 2023 7:15 PM
practicedTimes: 1
source: LC_Top_Interview_150, googleFreq

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

**Example 1:**

```
Input: s = "()"
Output: true

```

**Example 2:**

```
Input: s = "()[]{}"
Output: true

```

**Example 3:**

```
Input: s = "(]"
Output: false

```

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of parentheses only `'()[]{}'`.

```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char> stk;
        for(auto c: s){
            if(c=='(') {stk.push(')');}
            else if(c=='[') {stk.push(']');}
            else if(c=='{') {stk.push('}');}
            else if(stk.empty() || stk.top()!=c){
                return false;
            }
            else{stk.pop();}
        }
        
        return stk.empty();
    }
};
```