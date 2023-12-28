# Ones and Zeroes

#: 474
Difficult: Medium
Tags: dp-backpack
link: https://leetcode.com/problems/ones-and-zeroes/
Priority: High
Status: Dig More
Created time: January 2, 2023 10:02 PM
Last edited time: October 12, 2023 2:56 PM
source: wp动归套路

You are given an array of binary strings `strs` and two integers `m` and `n`.

Return *the size of the largest subset of `strs` such that there are **at most*** `m` **`0`*'s and* `n` **`1`*'s in the subset*.

A set `x` is a **subset** of a set `y` if all elements of `x` are also elements of `y`.

**Example 1:**

```
Input: strs = ["10","0001","111001","1","0"], m = 5, n = 3
Output: 4
Explanation: The largest subset with at most 5 0's and 3 1's is {"10", "0001", "1", "0"}, so the answer is 4.
Other valid but smaller subsets include {"0001", "1"} and {"10", "1", "0"}.
{"111001"} is an invalid subset because it contains 4 1's, greater than the maximum of 3.

```

**Example 2:**

```
Input: strs = ["10","0","1"], m = 1, n = 1
Output: 2
Explanation: The largest subset is {"0", "1"}, so the answer is 2.

```

**Constraints:**

- `1 <= strs.length <= 600`
- `1 <= strs[i].length <= 100`
- `strs[i]` consists only of digits `'0'` and `'1'`.
- `1 <= m, n <= 100`

```jsx
/*
to be improved with rotate array:
https://github.com/wisdompeak/LeetCode/blob/master/Dynamic_Programming/474.Ones-and-Zeroes/474.Ones-and-Zeroes.cpp
*/

/*
from combination -> backpack
dp[sz+1][m+1][n+1]: 
ijk: with 1..ith str, j0 and k1 is true

dpijk = dp i-1 j-cur0s, k-cur1s (check if valid)

for i j k

init dp000=true, other is false;
return dp.back().last == true
*/

class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        int sz=strs.size();
        strs.insert(strs.begin(),"");
        
        vector<vector<vector<int>>> dp(sz+1,vector<vector<int>>(m+1,vector<int>(n+1,0)));
        //dp[0][0][0]=0;
        
        for(int i=1; i<sz+1; i++){
            auto [zeroCnt,oneCnt] = get01s(strs[i]);
            for(int j=0; j<m+1; j++){//start from 0!!
                for(int k=0; k<n+1; k++){//start from 0!!
                    dp[i][j][k]=max(
                        dp[i-1][j][k],
                        (j-zeroCnt<0 || k-oneCnt<0) ? 0 :  
                            dp[i][j][k]=dp[i-1][j-zeroCnt][k-oneCnt]+1
                    );
                }
            }
        }
        
        return dp.back().back().back();
    }
    
    pair<int,int> get01s(string& s){
        int zeroCnt=0,oneCnt=0;
        for(auto& c: s){
            if(c=='0') {zeroCnt++;}
            else{oneCnt++;}
        }
        return {zeroCnt,oneCnt};
    }
};
```