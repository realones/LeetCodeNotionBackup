# Remove K Digits

#: 402
Difficult: Medium
Tags: Greedy, monotonic, stack, string
link: https://leetcode.com/problems/remove-k-digits/description/
Priority: High
Status: No Idea At First, toSummarize
Created time: December 13, 2023 11:51 PM
Last edited time: December 13, 2023 11:52 PM
source: microsoftFreq

Given string num representing a non-negative integer `num`, and an integer `k`, return *the smallest possible integer after removing* `k` *digits from* `num`.

**Example 1:**

```
Input: num = "1432219", k = 3
Output: "1219"
Explanation: Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.

```

**Example 2:**

```
Input: num = "10200", k = 1
Output: "200"
Explanation: Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes.

```

**Example 3:**

```
Input: num = "10", k = 2
Output: "0"
Explanation: Remove all the digits from the number and it is left with nothing which is 0.

```

**Constraints:**

- `1 <= k <= num.length <= 105`
- `num` consists of only digits.
- `num` does not have any leading zeros except for the zero itself.

```cpp
class Solution {
public:
    string removeKdigits(string num, int k) {
        int time=0;
        stack<int> stk;
        for(auto c: num){
            int x=c-'0';
            while(!stk.empty() && stk.top()>x && time<k) {
                stk.pop();
                time++;
            }
            stk.push(x);
        }
        string s="";
        while(!stk.empty()){
            s+=stk.top()+'0'; stk.pop();
        }
        while(!s.empty() && s.back()=='0') s.pop_back();//rm leading 0s
        reverse(s.begin(), s.end());
        for(;time<k;time++){
            if(!s.empty()) s.pop_back();
        }
        return s==""?"0":s;
    }
};

/*
1432219 k=3
1 432 
mono inc, if dec pop stk top, in total pop at most k times
if pop < k times, pop right ones till total k times
*/
```