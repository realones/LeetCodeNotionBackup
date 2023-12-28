# Ternary Expression Parser

#: 439
Difficult: Medium
Tags: recursion, stack, string
link: https://leetcode.com/problems/ternary-expression-parser/
Priority: Low
Status: More Solution
Created time: July 21, 2023 9:04 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

Given a string `expression` representing arbitrarily nested ternary expressions, evaluate the expression, and return *the result of it*.

You can always assume that the given expression is valid and only contains digits, `'?'`, `':'`, `'T'`, and `'F'` where `'T'` is true and `'F'` is false. All the numbers in the expression are **one-digit** numbers (i.e., in the range `[0, 9]`).

The conditional expressions group right-to-left (as usual in most languages), and the result of the expression will always evaluate to either a digit, `'T'` or `'F'`.

**Example 1:**

```
Input: expression = "T?2:3"
Output: "2"
Explanation: If true, then result is 2; otherwise result is 3.

```

**Example 2:**

```
Input: expression = "F?1:T?4:5"
Output: "4"
Explanation: The conditional expressions group right-to-left. Using parenthesis, it is read/evaluated as:
"(F ? 1 : (T ? 4 : 5))" --> "(F ? 1 : 4)" --> "4"
or "(F ? 1 : (T ? 4 : 5))" --> "(T ? 4 : 5)" --> "4"

```

**Example 3:**

```
Input: expression = "T?T?F:5:3"
Output: "F"
Explanation: The conditional expressions group right-to-left. Using parenthesis, it is read/evaluated as:
"(T ? (T ? F : 5) : 3)" --> "(T ? F : 3)" --> "F"
"(T ? (T ? F : 5) : 3)" --> "(T ? F : 5)" --> "F"

```

**Constraints:**

- `5 <= expression.length <= 104`
- `expression` consists of digits, `'T'`, `'F'`, `'?'`, and `':'`.
- It is **guaranteed** that `expression` is a valid ternary expression and that each number is a **one-digit number**.

Solution:

i think recursive solution is better than stack, since we only want to calculate either left part or right part, using stack seems more complex and calculating both left and right

```cpp
class Solution {
public:
    string parseTernary(string s) {
        if(s.size()==1) return s;

        /*
        find first ?
        left part must be signle T or F
        */
        char exp=s[0];
        s=s.substr(2);
        /*
        each ? mp to a :
        from first ?+1, find first : that is not map to left ? split to l and r, recursive find res
        */
        int i=0,cnt=0;
        for(; i<s.size(); i++){
            if(s[i]=='?') cnt++; 
            else if(s[i]==':') cnt--;
            if(cnt==-1) break;
        }
        string l=s.substr(0,i);
        string r=s.substr(i+1);

        if(exp=='T') return parseTernary(l);
        else return parseTernary(r);
        
        //todo: using start end to reduce space usage
        //todo: solution stk
    }
};

/*
? 
: 
T true
F false
[0~9] 

right to left

TorF  ?   A  :  B
A is sub
B is sub

return T/F/d

e.g.

T ?   T?1:2 : T?1:2
*/
```