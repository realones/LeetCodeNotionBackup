# Longest Valid Parentheses

#: 32
Difficult: Hard
Tags: DP, stack
link: https://leetcode.com/problems/longest-valid-parentheses/description/
Priority: Medium
Status: More Solution, No Idea At First
Created time: August 22, 2023 12:17 AM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

Given a string containing just the characters `'('` and `')'`, return *the length of the longest valid (well-formed) parentheses*

*substring*

.

**Example 1:**

```
Input: s = "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()".

```

**Example 2:**

```
Input: s = ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()".

```

**Example 3:**

```
Input: s = ""
Output: 0

```

**Constraints:**

- `0 <= s.length <= 3 * 104`
- `s[i]` is `'('`, or `')'`.

comment:

there are dp solution to check.

Solution - stk:

```cpp
using ci=pair<char,int>;

class Solution {
public:
    int longestValidParentheses(string s) {
        int n=s.size(); if(n==0) return 0;
        int res=0;
        stack<ci> stk;
        stk.emplace(')',-1);
        for(int i=0;i<n;i++){
            if(s[i]==')'){
                if(stk.top().first=='(') {
                    stk.pop();
                    res=max(res,i-stk.top().second);
                }
                else stk.emplace(')',i);
            }
            else{
                stk.emplace('(',i);
            }
        }
        return res;
    }
};
```