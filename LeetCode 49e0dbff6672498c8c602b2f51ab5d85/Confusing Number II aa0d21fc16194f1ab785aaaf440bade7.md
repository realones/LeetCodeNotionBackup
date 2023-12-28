# Confusing Number II

#: 1088
Difficult: Hard
Tags: BackTracking, Math
link: https://leetcode.com/problems/confusing-number-ii/
Priority: High
Status: toSummarize
Created time: October 10, 2022 2:07 AM
Last edited time: October 12, 2023 2:56 PM
source: googleFreq

A **confusing number** is a number that when rotated `180` degrees becomes a different number with **each digit valid**.

We can rotate digits of a number by `180` degrees to form new digits.

- When `0`, `1`, `6`, `8`, and `9` are rotated `180` degrees, they become `0`, `1`, `9`, `8`, and `6` respectively.
- When `2`, `3`, `4`, `5`, and `7` are rotated `180` degrees, they become **invalid**.

Note that after rotating a number, we can ignore leading zeros.

- For example, after rotating `8000`, we have `0008` which is considered as just `8`.

Given an integer `n`, return *the number of **confusing numbers** in the inclusive range* `[1, n]`.

**Example 1:**

```
Input: n = 20
Output: 6
Explanation: The confusing numbers are [6,9,10,16,18,19].
6 converts to 9.
9 converts to 6.
10 converts to 01 which is just 1.
16 converts to 91.
18 converts to 81.
19 converts to 61.

```

**Example 2:**

```
Input: n = 100
Output: 19
Explanation: The confusing numbers are [6,9,10,16,18,19,60,61,66,68,80,81,86,89,90,91,98,99,100].

```

**Constraints:**

- `1 <= n <= 109`

todo: summarize how to backtracking with for loop, refer to 

[https://leetcode.com/problems/confusing-number-ii/discuss/521261/C%2B%2B-Easy-to-understand-solution](https://leetcode.com/problems/confusing-number-ii/discuss/521261/C%2B%2B-Easy-to-understand-solution)

and

[https://www.notion.so/Maximum-Score-of-a-Node-Sequence-a517416f2e4740088d3b8ea38e1d1c13](Maximum%20Score%20of%20a%20Node%20Sequence%20a517416f2e4740088d3b8ea38e1d1c13.md)

todo: improvement with better solution

todo: why I need to check 1e9?

todo: check [https://leetcode.com/problems/confusing-number-ii/discuss/1365889/Python-Backtracking-Solution-Clear-explanation-Clean-and-Concise/1300088](https://leetcode.com/problems/confusing-number-ii/discuss/1365889/Python-Backtracking-Solution-Clear-explanation-Clean-and-Concise/1300088)

```cpp
//todo backtracking; write in loop
class Solution {
public:
    int confusingNumberII(int n) {
        int res=0;
        if(n==1e9) {n--,res++;}
        long long tmp=0;
        int kth=getDigitCnt(n);
        helper(n, res, tmp, kth);
        return res;
    }
    
    vector<int> candidate={0,1,6,8,9};
    void helper(int n, int& res, long long& tmp, int kth){
        if(tmp>n) return;
        if(kth==0) {
            if(valid(n,tmp)) res++;
            return;
        }
        for(auto d: candidate){
            int digit=getDigit(kth,d);
            tmp+=digit;
            helper(n,res,tmp,kth-1);
            tmp-=digit;
        }
    }
    
    vector<int> reverseCandidate={0,1,-1,-1,-1,-1,9,-1,8,6};
    bool valid(int n, long long num){
        if(num>n) return false;
        int reverseNum=0, tmp=num;
        while(tmp){
            reverseNum=reverseNum*10+reverseCandidate[tmp%10];
            tmp/=10;
        }
        return num!=reverseNum;
    }
    
    long long getDigit(int kth, int d){
        long long res=d;
        for(int i=0; i<kth-1; i++){
            res*=10;
        }
        return res;
    }
    
    int getDigitCnt(int n){
        int res=0;
        while(n){
            res++;
            n/=10;
        }
        return res;
    }
};

/*
combination backtracking

find cnt of digit in n

back tracking

helper: res, tmp, k

*/
```