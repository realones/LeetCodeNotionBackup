# Parsing A Boolean Expression

#: 1106
Difficult: Hard
Tags: recursion, stack, string
link: https://leetcode.com/problems/parsing-a-boolean-expression/
Priority: Medium
Created time: June 20, 2023 8:54 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

A **boolean expression** is an expression that evaluates to either `true` or `false`. It can be in one of the following shapes:

- `'t'` that evaluates to `true`.
- `'f'` that evaluates to `false`.
- `'!(subExpr)'` that evaluates to **the logical NOT** of the inner expression `subExpr`.
- `'&(subExpr1, subExpr2, ..., subExprn)'` that evaluates to **the logical AND** of the inner expressions `subExpr1, subExpr2, ..., subExprn` where `n >= 1`.
- `'|(subExpr1, subExpr2, ..., subExprn)'` that evaluates to **the logical OR** of the inner expressions `subExpr1, subExpr2, ..., subExprn` where `n >= 1`.

Given a string `expression` that represents a **boolean expression**, return *the evaluation of that expression*.

It is **guaranteed** that the given expression is valid and follows the given rules.

**Example 1:**

```
Input: expression = "&(|(f))"
Output: false
Explanation:
First, evaluate |(f) --> f. The expression is now "&(f)".
Then, evaluate &(f) --> f. The expression is now "f".
Finally, return false.

```

**Example 2:**

```
Input: expression = "|(f,f,f,t)"
Output: true
Explanation: The evaluation of (false OR false OR false OR true) is true.

```

**Example 3:**

```
Input: expression = "!(&(f,t))"
Output: true
Explanation:
First, evaluate &(f,t) --> (false AND true) --> false --> f. The expression is now "!(f)".
Then, evaluate !(f) --> NOT false --> true. We return true.

```

**Constraints:**

- `1 <= expression.length <= 2 * 104`
- expression[i] is one following characters: `'('`, `')'`, `'&'`, `'|'`, `'!'`, `'t'`, `'f'`, and `','`.

```cpp
class Solution {
public:
    bool parseBoolExpr(string expression) {
        stack<char> stk;
        for(auto c: expression){
            if(c=='('){ stk.push(c); }
            else if(c==')'){
                vector<char> arr;
                while(stk.top()=='t' || stk.top()=='f'){
                    arr.push_back(stk.top()=='t'?1:0);
                    stk.pop();
                }
                if(stk.top()=='(') stk.pop(); //pop ( //check case e.g. !t
                char op=stk.top(); stk.pop();
                if(op!='&' && op!='|' && op!='!') std::cout << "error_)_op" << std::endl;
                std::cout << "op" << " -- " << op << std::endl;
                std::cout<< "arr" <<" -- "; std::copy(arr.begin(), arr.end(), std::ostream_iterator<int>(std::cout," ")); std::cout<<std::endl;

                int max=*max_element(arr.begin(), arr.end());
                int min=*min_element(arr.begin(), arr.end());
                if(op=='&'){stk.push(min==0?'f':'t');}
                else if(op=='|'){stk.push(max==1?'t':'f');}
                else if(op=='!'){stk.push(min==0?'t':'f');}
                else cout << ")_op_error" << std::endl;
            }
            else if(c=='&'){ stk.push(c); }
            else if(c=='|'){ stk.push(c); }
            else if(c=='!'){ stk.push(c); }
            else if(c=='t'){ stk.push(c); }
            else if(c=='f'){ stk.push(c); }
            else if(c==','){ continue; }
            else{std::cout << "c_error" << std::endl;}
        }
        return stk.top()=='t'?true:false;
    }
};

/*
stk:

'(' in stk

')' de t/f till (
    de operator
    calcu
    in stk

'&' in stk
'|' 
'!'

't' in stk
'f'

',' - skip

return stk.top()
*/
```