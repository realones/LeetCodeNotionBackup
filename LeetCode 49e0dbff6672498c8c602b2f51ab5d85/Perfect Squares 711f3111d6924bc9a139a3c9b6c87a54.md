# Perfect Squares

#: 279
Difficult: Medium
Tags: BFS, Math, dp-backpack
link: https://leetcode.com/problems/perfect-squares/
Priority: High
Status: More Solution, No Idea At First
Created time: September 21, 2023 7:37 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given an integer `n`, return *the least number of perfect square numbers that sum to* `n`.

A **perfect square** is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example, `1`, `4`, `9`, and `16` are perfect squares while `3` and `11` are not.

**Example 1:**

```
Input: n = 12
Output: 3
Explanation: 12 = 4 + 4 + 4.

```

**Example 2:**

```
Input: n = 13
Output: 2
Explanation: 13 = 4 + 9.

```

**Constraints:**

- `1 <= n <= 104`

Solution old:

```cpp
class Solution {
public:
    //similar to Count Primes, build array with size n+1
    //but use int instead of bool ==> DP
    
    //f(k) result for K
    //f(k) => update all f(t) that t=k+[square num] if smaller
    //f(0) = 0
    //f(n) return
    int numSquares(int n) {
        vector<int> dp(n+1,INT_MAX);
        dp[0]=0;
        for(int i=0;i<=n;i++){
            for(int j=1; i+j*j<=n;j++){
                dp[i+j*j]=min(dp[i+j*j], dp[i]+1);
            }
        }
        return dp[n];
    }
   
};
```

Solution backpack for take any: 2d → 1d:

```cpp
class Solution {
public:
    int numSquares(int n) {
        //prepare square numbers
        vector<int> nums;
        for(int i=1; i<=floor(sqrt(n)); i++){
            nums.push_back(pow(i,2));
        }
        //dp-backpack 2d->1d, since take any
        vector<int> dp(n+1,INT_MAX/2);
        //init
        dp[0]=0;
        for(int s=1; s<=n; s++){//for each sum
            for(auto x: nums){//for each square number
                dp[s]=min(dp[s],s-x>=0?dp[s-x]+1:INT_MAX/2);
            }
        }
        return dp.back();
    }
}
```

Solution dp-backpack - TLE:

T : O(1e4 * 100 * 100)
S : O(1e4 * 100)

```cpp
class Solution {
public:
    int numSquares(int n) {
        //prepare square numbers
        vector<int> nums;
        for(int i=1; i<=floor(sqrt(n)); i++){
            nums.push_back(pow(i,2));
        }
        //dp-backpack
        vector<vector<int>> dp(nums.size()+1,vector<int>(n+1,INT_MAX/2));
        //init
        dp[0][0]=0;
        //process
        for(int i=1;i<nums.size()+1;i++){
            int cur=nums[i-1];
            for(int j=0;j<n+1;j++){
                for(int k=0;j-k*cur>=0;k++){
                    dp[i][j]=min(dp[i][j], dp[i-1][j-k*cur]+k);
                }
            }
        }
        return dp.back().back();
    }
};

/*
n: 1~1e4
find min cnt of perfect square numbers, that sum of them == n

greedy? --> always find the largest square number?
    13 - 9
    26 - 25

1 4 9 16 25 36 49 64 81 100 121
12 = 4 4 4
12 = 9 1 1 1

dp? find all possible solutions

12
1 - 11
4 - 8
9 - 3

f(n) = min(n-a square number) + 1

possible squares
1 2 3 4 ... floor(sqrt(n))
at most floor(sqrt(1e4)) = 100 squares
then for each number, pow by 2

f(n) = min(n-a square number) + 1

T : O(1e4 * 100 * 100)
S : O(1e4 * 100)

================================
dp-backpack

dp[first ith square numbers:0~lastSquareNumber's idx][0~n]

dp[i][j] min= 
    use         dp[i-1][j-cur*k]+k

init:
    dp[0][0]=0; other INT_MAX

return:
    dp.back().back();
*/
```