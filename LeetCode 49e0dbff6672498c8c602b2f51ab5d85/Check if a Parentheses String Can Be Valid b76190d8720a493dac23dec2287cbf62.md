# Check if a Parentheses String Can Be Valid

#: 2116
Difficult: Medium
Tags: Greedy, stack, string
link: https://leetcode.com/problems/check-if-a-parentheses-string-can-be-valid/description/
Priority: High
Status: No Idea At First
Created time: December 21, 2023 10:17 PM
Last edited time: December 21, 2023 10:17 PM
source: googleFreq

A parentheses string is a **non-empty** string consisting only of `'('` and `')'`. It is valid if **any** of the following conditions is **true**:

- It is `()`.
- It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid parentheses strings.
- It can be written as `(A)`, where `A` is a valid parentheses string.

You are given a parentheses string `s` and a string `locked`, both of length `n`. `locked` is a binary string consisting only of `'0'`s and `'1'`s. For **each** index `i` of `locked`,

- If `locked[i]` is `'1'`, you **cannot** change `s[i]`.
- But if `locked[i]` is `'0'`, you **can** change `s[i]` to either `'('` or `')'`.

Return `true` *if you can make `s` a valid parentheses string*. Otherwise, return `false`.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/11/06/eg1.png](https://assets.leetcode.com/uploads/2021/11/06/eg1.png)

```
Input: s = "))()))", locked = "010100"
Output: true
Explanation: locked[1] == '1' and locked[3] == '1', so we cannot change s[1] or s[3].
We change s[0] and s[4] to '(' while leaving s[2] and s[5] unchanged to make s valid.
```

**Example 2:**

```
Input: s = "()()", locked = "0000"
Output: true
Explanation: We do not need to make any changes because s is already valid.

```

**Example 3:**

```
Input: s = ")", locked = "0"
Output: false
Explanation: locked permits us to change s[0].
Changing s[0] to either '(' or ')' will not make s valid.

```

**Constraints:**

- `n == s.length == locked.length`
- `1 <= n <= 105`
- `s[i]` is either `'('` or `')'`.
- `locked[i]` is either `'0'` or `'1'`.

```cpp
class Solution {
public:
    bool canBeValid(string s, string locked) {
        int n=s.size();
        for(int i=0;i<n;i++){
            if(locked[i]=='0') s[i]='*';
        }
        auto f = [&](string& s){
            int diff=0, wild=0;
            for(auto c: s){
                if(c=='('){
                    diff++;
                }
                else if(c==')'){
                    diff--;
                    if(diff<0){
                        if(wild>0) wild--, diff++;
                        else return false;
                    }
                }
                else if(c=='*'){
                    wild++;
                }
                else std::cout << "error" << std::endl;
            }
            return (wild-diff>=0) && (wild-diff)% 2==0;
        };
        string reS=s;
        reverse(reS.begin(), reS.end());
        for(auto& c:reS){
            if(c=='(') c=')';
            else if(c==')') c='(';
        }
        return f(s) && f(reS);
    }
};

/*
define diff = cnt"(" - cnt")"
could be multi backtracking search, but can maintain the max min instead of detailed diff

=====w/o locked
for each char
    if ( diff++
    if ) diff--
    if(diff<0) return false
return diff==0
T: O(n)

=====w/ locked
for each char
    if(unlocked){
        to ( diff++ , maintain a set of cur result? -> overall t: O(2^n) - combination - TLE
        to ) diff-- ,if(diff<0) dont do it
    }
    if(diff<0) return false
return diff==0

================================
hint:
    odd str: invalid
    left -> right:
        see locked ) -> must be balanced with (, either from locked or unlocked
*/
```