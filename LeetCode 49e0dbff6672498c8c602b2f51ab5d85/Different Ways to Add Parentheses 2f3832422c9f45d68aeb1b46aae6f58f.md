# Different Ways to Add Parentheses

#: 241
Difficult: Medium
Tags: DP, Math, recursion, string
link: https://leetcode.com/problems/different-ways-to-add-parentheses/description/
Priority: Medium
Status: More Solution
Created time: June 27, 2023 11:42 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given a string `expression` of numbers and operators, return *all possible results from computing all the different possible ways to group numbers and operators*. You may return the answer in **any order**.

The test cases are generated such that the output values fit in a 32-bit integer and the number of different results does not exceed `104`.

**Example 1:**

```
Input: expression = "2-1-1"
Output: [0,2]
Explanation:
((2-1)-1) = 0
(2-(1-1)) = 2

```

**Example 2:**

```
Input: expression = "2*3-4*5"
Output: [-34,-14,-10,-10,10]
Explanation:
(2*(3-(4*5))) = -34
((2*3)-(4*5)) = -14
((2*(3-4))*5) = -10
(2*((3-4)*5)) = -10
(((2*3)-4)*5) = 10

```

**Constraints:**

- `1 <= expression.length <= 20`
- `expression` consists of digits and the operator `'+'`, `'-'`, and `'*'`.
- All the integer values in the input expression are in the range `[0, 99]`.

comment:

一开始陷入idea1（in comment），不好实现。

这个solution有很多地方可以improve

```cpp
class Solution {
public:
    //todo improve with passing string& with start and end, use DP for memo
    vector<int> diffWaysToCompute(string expression) {
        if(expression.find_first_of("*-+")==-1) return {stoi(expression)};
        vector<int> res;
        for(int i=0;i<expression.size();i++){
            char c=expression[i];
            if(c=='+' || c=='-' || c=='*'){
                string l=expression.substr(0,i);
                string r=expression.substr(i+1);
                vector<int> l_res=(diffWaysToCompute(l));
                vector<int> r_res=(diffWaysToCompute(r));
                for(auto a: l_res){
                    for(auto b: r_res){
                        int calcu=0;
                        if(c=='+'){ calcu=a+b; }
                        else if(c=='-'){ calcu=a-b; }
                        else{ calcu=a*b; }
                        res.push_back(calcu);
                    }
                }
            }
        }
        return res;
    }
};
/*
idea1: diff to implement
calcu permutation of operator execution sequence
for each permutation, calculate the result and add to hash_set

idea2: from solutions answer
for each operator
    l is a vector<int> of possible result from left substr
    r is a vector<int> of possible result from right substr
    return combined (l op r)
*/
```