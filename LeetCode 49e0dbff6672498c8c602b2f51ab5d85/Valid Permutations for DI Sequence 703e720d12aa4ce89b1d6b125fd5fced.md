# Valid Permutations for DI Sequence

#: 903
Difficult: Hard
Tags: Permutation, dp-TimeSeqFromLastOne-Power
link: https://leetcode.com/problems/valid-permutations-for-di-sequence/
Priority: High
Status: Dig More, No Idea At First
Created time: January 27, 2023 4:08 PM
Last edited time: October 12, 2023 2:56 PM
source: wp动归套路

You are given a string `s` of length `n` where `s[i]` is either:

- `'D'` means decreasing, or
- `'I'` means increasing.

A permutation `perm` of `n + 1` integers of all the integers in the range `[0, n]` is called a **valid permutation** if for all valid `i`:

- If `s[i] == 'D'`, then `perm[i] > perm[i + 1]`, and
- If `s[i] == 'I'`, then `perm[i] < perm[i + 1]`.

Return *the number of **valid permutations*** `perm`. Since the answer may be large, return it **modulo** `109 + 7`.

**Example 1:**

```
Input: s = "DID"
Output: 5
Explanation: The 5 valid permutations of (0, 1, 2, 3) are:
(1, 0, 3, 2)
(2, 0, 3, 1)
(2, 1, 3, 0)
(3, 0, 2, 1)
(3, 1, 2, 0)

```

**Example 2:**

```
Input: s = "D"
Output: 1

```

**Constraints:**

- `n == s.length`
- `1 <= n <= 200`
- `s[i]` is either `'I'` or `'D'`.

**Solution**:

```jsx
/*
========================
dp definition:
 #DIDI
[XXXXj]
     i

dp[i][j]: 
	how many valid permutation of [0:i], s.t. the i-th element is j (j in the range of [0:i])

dp[i][j] = 
	Increase: sum of dp[i-1][0], dp[i-1][1], ..., dp[i-1][j-1]
	Decrease: sum of dp[i-1][j], dp[i-1][j+1], ..., dp[i-1][i-1]

init:
	dp[0][0] = 1

return:
	sum of dp[n][?]
========================
整体抬高:
0312 + 3 
if element >= j, element++
0412 + 3
->
04123
========================
T: 200*200*200
*/

class Solution {
public:
    int M=1e9+7;
    //s: D or I
    //size<=200
    int numPermsDISequence(string s) {
        int n=s.size();
        s.insert(s.begin(),'#');
        
        vector<vector<int>> dp(n+1,vector<int>(n+1,0));
        dp[0][0]=1;
        
        for(int i=1;i<=n;i++){
            for(int j=0;j<=i;j++){
                if(s[i]=='I'){
                    for(int k=0;k<=j-1;k++){
                        dp[i][j]+=dp[i-1][k];
                        dp[i][j]%=M;
                    }
                }
                else{//'D'
                    for(int k=j;k<=i-1;k++){
                        dp[i][j]+=dp[i-1][k];
                        dp[i][j]%=M;
                    }
                }
            }
        }
        
        int res=0;
        for(auto num: dp[n]){
            res+=num;
            res%=M;
        }
        return res;
    }
};
```