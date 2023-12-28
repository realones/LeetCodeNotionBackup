# Longest Happy String

#: 1405
Difficult: Medium
Tags: Greedy, PQ, recursion, string
link: https://leetcode.com/problems/longest-happy-string/description/
Priority: High
Status: More Solution, No Idea At First, toRevisit
Created time: December 14, 2023 9:58 PM
Last edited time: December 14, 2023 10:36 PM
source: microsoftFreq

A string `s` is called **happy** if it satisfies the following conditions:

- `s` only contains the letters `'a'`, `'b'`, and `'c'`.
- `s` does not contain any of `"aaa"`, `"bbb"`, or `"ccc"` as a substring.
- `s` contains **at most** `a` occurrences of the letter `'a'`.
- `s` contains **at most** `b` occurrences of the letter `'b'`.
- `s` contains **at most** `c` occurrences of the letter `'c'`.

Given three integers `a`, `b`, and `c`, return *the **longest possible happy** string*. If there are multiple longest happy strings, return *any of them*. If there is no such string, return *the empty string* `""`.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

```
Input: a = 1, b = 1, c = 7
Output: "ccaccbcc"
Explanation: "ccbccacc" would also be a correct answer.

```

**Example 2:**

```
Input: a = 7, b = 1, c = 0
Output: "aabaa"
Explanation: It is the only correct answer in this case.

```

**Constraints:**

- `0 <= a, b, c <= 100`
- `a + b + c > 0`

```cpp
using ic=pair<int,char>;

class Solution {
public:
    string longestDiverseString(int a, int b, int c) {
        vector<ic> v={{a,'a'}, {b,'b'}, {c,'c'}};
        string res="";
        while(true){
            sort(v.begin(), v.end(), greater<>());
            auto& [cnt1,c1]=v[0];
            auto& [cnt2,c2]=v[1];
            auto& [cnt3,c3]=v[2];
            //1>=2>=3
            //try use cnt1
            if(cnt1>=2){
                res+=string(2,c1);
                cnt1-=2;
            }
            else if(cnt1==1){
                res+=c1;
                cnt1--;
            }
            else break;
            //try use cnt2
            if(cnt2>0){
                if(cnt1>=cnt2){
                    res+=c2;
                    cnt2--;
                }
            }
            else break;
        }
        return res;
    }
};

/*
votrubac: greedy

intui:
in each round
    Aassuming a >= b >= c: 
    ignore the smallest count: c
    add 'aa'. 
    add 'b' 
Repeat recursivelly for the remaining counts.
*/
```