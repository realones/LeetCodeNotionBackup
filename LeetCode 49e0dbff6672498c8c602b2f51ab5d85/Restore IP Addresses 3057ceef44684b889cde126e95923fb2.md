# Restore IP Addresses

#: 93
Difficult: Medium
Tags: BackTracking, DFS, string
link: https://leetcode.com/problems/restore-ip-addresses/description/
Priority: Medium
Status: HardToImplement, toSummarize
Created time: August 19, 2023 5:33 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Split Array into Fibonacci Sequence (Split%20Array%20into%20Fibonacci%20Sequence%20c4ae21c2788c414a97bb326e8c8a2b86.md)

A **valid IP address** consists of exactly four integers separated by single dots. Each integer is between `0` and `255` (**inclusive**) and cannot have leading zeros.

- For example, `"0.1.2.201"` and `"192.168.1.1"` are **valid** IP addresses, but `"0.011.255.245"`, `"192.168.1.312"` and `"192.168@1.1"` are **invalid** IP addresses.

Given a string `s` containing only digits, return *all possible valid IP addresses that can be formed by inserting dots into* `s`. You are **not** allowed to reorder or remove any digits in `s`. You may return the valid IP addresses in **any** order.

**Example 1:**

```
Input: s = "25525511135"
Output: ["255.255.11.135","255.255.111.35"]

```

**Example 2:**

```
Input: s = "0000"
Output: ["0.0.0.0"]

```

**Example 3:**

```
Input: s = "101023"
Output: ["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]

```

**Constraints:**

- `1 <= s.length <= 20`
- `s` consists of digits only.

comment:

implementation is a bit hard than i expected to be very straight forward

check relationship with K-ranges dp, and build a pattern for the recursive version

solution:

```cpp
class Solution {
public:
    vector<string> res;
    vector<string> tmp;

    vector<string> restoreIpAddresses(string s) {
        helper(s,4);
        return res;
    }

    //todo back tracking and use string& s with start, end, instead of cutting s
    void helper(string s, int cnt){
        if(cnt==0 && s.empty()) {res.push_back(toAddr(tmp)); return;}
        if(cnt==0 || s.empty()) return;
        for(int len=1;len<=min((int)s.size(),3);len++){
            string l=s.substr(0,len);
            string r=s.substr(len);
            if(valid(l)) {
                tmp.push_back(l);
                helper(r,cnt-1);
                tmp.pop_back();
            }
        }
    }

    //todo use ref
    string toAddr(vector<string> tmp){
        string res;//todo handle it gracefully
        for(auto s: tmp) res+=s+".";
        res.pop_back();
        return res;
    }

    //todo use ref
    bool valid(string s){
        if(s.empty()) return false;
        if(s.size()>1) if(s.front()=='0') return false;
        return stoi(s)>=0 && stoi(s)<=255;
    }
};
/*
backtracking to insert three dot into s, mean while keep it valid
use vector<string> to represent an address to simplify
not using backtracking but dfs only to start to simplify

get first piece, then recur(rest of s, len-1), len init as 4
if len==0, add to res
*
```