# Split Array into Fibonacci Sequence

#: 842
Difficult: Medium
Tags: BackTracking, string
link: https://leetcode.com/problems/split-array-into-fibonacci-sequence/description/
Priority: Medium
Status: HardToImplement
Created time: June 27, 2023 11:58 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Restore IP Addresses (Restore%20IP%20Addresses%203057ceef44684b889cde126e95923fb2.md)

You are given a string of digits `num`, such as `"123456579"`. We can split it into a Fibonacci-like sequence `[123, 456, 579]`.

Formally, a **Fibonacci-like** sequence is a list `f` of non-negative integers such that:

- `0 <= f[i] < 231`, (that is, each integer fits in a **32-bit** signed integer type),
- `f.length >= 3`, and
- `f[i] + f[i + 1] == f[i + 2]` for all `0 <= i < f.length - 2`.

Note that when splitting the string into pieces, each piece must not have extra leading zeroes, except if the piece is the number `0` itself.

Return any Fibonacci-like sequence split from `num`, or return `[]` if it cannot be done.

**Example 1:**

```
Input: num = "1101111"
Output: [11,0,11,11]
Explanation: The output [110, 1, 111] would also be accepted.

```

**Example 2:**

```
Input: num = "112358130"
Output: []
Explanation: The task is impossible.

```

**Example 3:**

```
Input: num = "0123"
Output: []
Explanation: Leading zeroes are not allowed, so "01", "2", "3" is not valid.

```

**Constraints:**

- `1 <= num.length <= 200`
- `num` contains only digits.

Solution:

backtracking 我总是有点晕

```cpp
using ll=long long;

class Solution {
public:
    //todo improve with start, end and string&
    vector<int> res;
    vector<ll> tmp;

    vector<int> splitIntoFibonacci(string num) {
        if(num.size()<3) return {};
        helper(num);
        return res;
    }

    bool helper(string num) {
        if(num.empty() && tmp.size()>=3) {res=vector<int>(tmp.begin(), tmp.end()); return true;}
        if(num.empty()) return false;

        for(int len=1;len<=min(10,(int)num.size());len++){
            string l=num.substr(0,len);
            string r=num.substr(len);
            if(valid(l)){
                tmp.push_back(stoll(l));
                if(helper(r)) return true;
                tmp.pop_back();
            }
        }
        return false;
    }

    bool valid(string s){
        if(s.size()>1 && s.front()=='0') return false;//move check in helper to early exit
        ll val=stoll(s);
        if(val>INT_MAX) return false;
        if(tmp.size()>=2 && val != tmp.back() + *next(tmp.rbegin())) return false;
        return true;
    }
}
```