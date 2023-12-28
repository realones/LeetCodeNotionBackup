# Minimum Add to Make Parentheses Valid

#: 921
Difficult: Medium
Tags: Greedy, stack, string
link: https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/description/
Priority: Low
Created time: December 3, 2023 1:46 AM
Last edited time: December 3, 2023 1:49 AM
source: facebookFreq

A parentheses string is valid if and only if:

- It is the empty string,
- It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid strings, or
- It can be written as `(A)`, where `A` is a valid string.

You are given a parentheses string `s`. In one move, you can insert a parenthesis at any position of the string.

- For example, if `s = "()))"`, you can insert an opening parenthesis to be `"(**(**)))"` or a closing parenthesis to be `"())**)**)"`.

Return *the minimum number of moves required to make* `s` *valid*.

**Example 1:**

```
Input: s = "())"
Output: 1

```

**Example 2:**

```
Input: s = "((("
Output: 3

```

**Constraints:**

- `1 <= s.length <= 1000`
- `s[i]` is either `'('` or `')'`.

Solution w/o stk:

```cpp
class Solution {
public:
    int minAddToMakeValid(string s) {
        int l=0,r=0;
        for(auto c: s){
            if(c=='(') l++;
            else{
                if(l>0) l--;
                else r++;
            }
        }
        return l+r;
    }
};
```

solution stk:

```cpp
class Solution {
public:
    int minAddToMakeValid(string s) {
        stack<char> stk;
        for(auto c: s){
            if(c=='(') stk.push(c);
            if(c==')') {
                if(!stk.empty() && stk.top()=='(') stk.pop();
                else stk.push(c);
            }
        }
        return stk.size();
    }
};
```