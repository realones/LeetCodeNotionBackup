# Expression Add Operators

#: 282
Difficult: Hard
Tags: BackTracking, Math, string
link: https://leetcode.com/problems/expression-add-operators/description/
Priority: High
Status: Dig More, HardToImplement
Created time: June 27, 2023 11:59 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given a string `num` that contains only digits and an integer `target`, return ***all possibilities** to insert the binary operators* `'+'`*,* `'-'`*, and/or* `'*'` *between the digits of* `num` *so that the resultant expression evaluates to the* `target` *value*.

Note that operands in the returned expressions **should not** contain leading zeros.

**Example 1:**

```
Input: num = "123", target = 6
Output: ["1*2*3","1+2+3"]
Explanation: Both "1*2*3" and "1+2+3" evaluate to 6.

```

**Example 2:**

```
Input: num = "232", target = 8
Output: ["2*3+2","2+3*2"]
Explanation: Both "2*3+2" and "2+3*2" evaluate to 8.

```

**Example 3:**

```
Input: num = "3456237490", target = 9191
Output: []
Explanation: There are no expressions that can be created from "3456237490" to evaluate to 9191.

```

**Constraints:**

- `1 <= num.length <= 10`
- `num` consists of only digits.
- `231 <= target <= 231 - 1`

comment:

idea is simple, just brute force and gather the correct result, however, the 

super hard to figure out the implementation.. take long time to write the code

自己想出来的，需要看看别人答案

```cpp
using ll=long long;

class Solution {
public:
    vector<ll> num;
    string numStr;
    ll target;
    vector<string> res;
    string tmp;

    vector<string> addOperators(string numStr, int target) {
        for(auto c: numStr) num.push_back(c-'0');
        this->numStr=numStr;
        this->target=target;
        this->tmp+=numStr[0];

        backtracking(1,num[0],0,1,1);
        return res;
    }

    /*
        idx: 1..n-1, means the gap between num[i-1] and num[i]
        cur: init as num[0]
        toAdd: init as 0
        toMul: init as 1
        sign: init as 1

        when touch end: idx=n: sum=toAdd + toMul*cur
        facing:
            + toAdd = toAdd + toMul*cur, cur=0, toMul=1, sign=1
            - toAdd = toAdd + toMul*cur, cur=0, toMul=1, sign=-1
            * toMul = toMul*cur, cur=0, sign=1
            # cur   = cur append num[idx], take care of negtive cur
    */
    void backtracking(ll idx, ll cur, ll toAdd, ll toMul, ll sign){
        if(idx==num.size()) {
            //std::cout << "tmp" << " -- " << tmp << std::endl;
            if(target == toAdd + toMul*cur) res.push_back(tmp);
            return;
        };
 
        //+
        tmp+='+',tmp+=numStr[idx];
        backtracking(idx+1, num[idx], toAdd + toMul*cur, 1, 1);
        tmp.pop_back();tmp.pop_back();
        //-
        tmp+='-',tmp+=numStr[idx];
        backtracking(idx+1, -num[idx], toAdd + toMul*cur, 1, -1);
        tmp.pop_back();tmp.pop_back();
        //*
        tmp+='*',tmp+=numStr[idx];
        backtracking(idx+1, num[idx], toAdd, toMul*cur, 1);
        tmp.pop_back();tmp.pop_back();
        //#
        if(cur!=0){
            tmp+=numStr[idx];
            backtracking(idx+1, (abs(cur)*10+num[idx])*sign, toAdd, toMul, sign);
            tmp.pop_back();
        }
    }
};

/*
n-1 gaps, 4 options +-*#, 4^(n-1)
*/
```