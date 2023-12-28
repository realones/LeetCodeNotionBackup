# Count of Integers

#: 2719
Difficult: Hard
Tags: DP, Math, Prefix, string
link: https://leetcode.com/problems/count-of-integers/
Priority: High
Status: No Idea At First
Created time: June 3, 2023 9:21 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given two numeric strings `num1` and `num2` and two integers `max_sum` and `min_sum`. We denote an integer `x` to be *good* if:

- `num1 <= x <= num2`
- `min_sum <= digit_sum(x) <= max_sum`.

Return *the number of good integers*. Since the answer may be large, return it modulo `1e9 + 7`.

Note that `digit_sum(x)` denotes the sum of the digits of `x`.

**Example 1:**

```
Input: num1 = "1", num2 = "12", min_num = 1, max_num = 8
Output: 11
Explanation: There are 11 integers whose sum of digits lies between 1 and 8 are 1,2,3,4,5,6,7,8,10,11, and 12. Thus, we return 11.

```

**Example 2:**

```
Input: num1 = "1", num2 = "5", min_num = 1, max_num = 5
Output: 5
Explanation: The 5 integers whose sum of digits lies between 1 and 5 are 1,2,3,4, and 5. Thus, we return 5.

```

**Constraints:**

- `1 <= num1 <= num2 <= 1e22`
- `1 <= min_sum <= max_sum <= 400`

Solution

```cpp
using ll = long long;
ll M=1e9+7;

class Solution {
public:
    int count(string num1, string num2, int min_sum, int max_sum) {
        ll res = (cntNoGreater(num2,max_sum) - cntNoGreater(num2,min_sum-1) + M) %M
                    -
                  (cntNoGreater(num1,max_sum) - cntNoGreater(num1,min_sum-1) + M) %M;
        res=(res+M)%M;
        
        //num1 case
        int digitSum=0;
        for(auto c: num1){
            digitSum+=c-'0';
        }

        if(min_sum<=digitSum && digitSum<=max_sum) return (res+1)%M;
        return res;
    }

    ll cntNoGreater(string& num, int max_sum){
        vector<vector<vector<int>>> dp(25,vector<vector<int>>(2,vector<int>(405,-1)));
        return dfs(num, max_sum, 0, true, 0, dp);
    }

    ll dfs(string& num, int max_sum, int idx, bool isSame, int sum, vector<vector<vector<int>>>& dp){
        if(sum>max_sum) return 0;
        if(dp[idx][isSame][sum]!=-1) return dp[idx][isSame][sum];
        if(idx==num.size()) return 1;

        int res=0;
        if(!isSame){
            for(int k=0; k<=9; k++){
                res+=dfs(num, max_sum, idx+1, false, sum+k,dp);
                res%=M;
            }
        }
        else{
            for(int k=0; k<num[idx]-'0'; k++){
                res+=dfs(num, max_sum, idx+1, false, sum+k,dp);
                res%=M;
            }
            res+=dfs(num, max_sum, idx+1, true, sum+num[idx]-'0', dp);
            res%=M;
        }

        dp[idx][isSame][sum] = res;
        return dp[idx][isSame][sum];
    }
};

/*
**Intuition**
prefixSum like func: - NoGreaterThan
    calcu [0,strNum], count of integers with digit sum in [0, max_sum]

**Explanation**
using above func():
    get cnt1= (num1-1 , max_sum) - (num1-1 , min_sum-1)
    get cnt2= (num2   , max_sum) - (num2   , min_sum-1)
return cnt2-cnt1

func - get cnt by (num, sum):
Intuition:
    recursive from left to right, abstract the digit
Implemention:
(num, sum, curDigitIdx:l->r -init:l, isSame, curSum) -- dp[idx][isSame][curSum]
    for d: 0~9           if left ones not same
    for d: 0~d[from num] if left ones same

**Complexity**
T:
S:

*/
```