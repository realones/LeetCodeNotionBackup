# Put Marbles in Bags

#: 2551
Difficult: Hard
Tags: Array, Greedy, PQ, Sort
link: https://leetcode.com/problems/put-marbles-in-bags/
Priority: High
Status: In Progress, No Idea At First
Created time: July 7, 2023 5:22 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

You have `k` bags. You are given a **0-indexed** integer array `weights` where `weights[i]` is the weight of the `ith` marble. You are also given the integer `k.`

Divide the marbles into the `k` bags according to the following rules:

- No bag is empty.
- If the `ith` marble and `jth` marble are in a bag, then all marbles with an index between the `ith` and `jth` indices should also be in that same bag.
- If a bag consists of all the marbles with an index from `i` to `j` inclusively, then the cost of the bag is `weights[i] + weights[j]`.

The **score** after distributing the marbles is the sum of the costs of all the `k` bags.

Return *the **difference** between the **maximum** and **minimum** scores among marble distributions*.

**Example 1:**

```
Input: weights = [1,3,5,1], k = 2
Output: 4
Explanation:
The distribution [1],[3,5,1] results in the minimal score of (1+1) + (3+1) = 6.
The distribution [1,3],[5,1], results in the maximal score of (1+3) + (5+1) = 10.
Thus, we return their difference 10 - 6 = 4.

```

**Example 2:**

```
Input: weights = [1, 3], k = 2
Output: 0
Explanation: The only distribution possible is [1],[3].
Since both the maximal and minimal score are the same, we return 0.

```

**Constraints:**

- `1 <= k <= weights.length <= 105`
- `1 <= weights[i] <= 109`

Solution kranges dp, **MLE**:

```cpp
using ll=long long;

class Solution {
public:
    long long putMarbles(vector<int>& weights, int k) {
        return findMax(weights,k) - findMin(weights,k);
    }

    long long findMax(vector<int>& weights, int K) {
        int n=weights.size();
        vector<vector<ll>> dp(n,vector<ll>(K+1,INT_MIN/2));//assign invalid
        for(int i=0;i<n;i++){
            for(int k=1;k<=min(i+1,K);k++){
                for(int j=i;j>=k-1;j--){
                    ll costji=weights[i]+weights[j];
                    dp[i][k]=max(dp[i][k], (j-1>=0?dp[j-1][k-1]:0) + costji);
                }
            }
        }
        return dp.back().back();
    }

    long long findMin(vector<int>& weights, int K) {
        int n=weights.size();
        vector<vector<ll>> dp(n,vector<ll>(K+1,INT_MAX/2));//assign invalid
        for(int i=0;i<n;i++){
            for(int k=1;k<=min(i+1,K);k++){
                for(int j=i;j>=k-1;j--){
                    ll costji=weights[i]+weights[j];
                    dp[i][k]=min(dp[i][k], (j-1>=0?dp[j-1][k-1]:0) + costji);
                }
            }
        }
        return dp.back().back();
    }
};

/*
divied weights to k ranges, (k:1~wlen, wlen:1~1e5, w:1~1e9)
each range sz>=1
range cost= w[range_start]+w[range_end], inclusive
score=sum of all bag costs
return: max_score - min_score
*/

/*
intui:
divide k to k-1 and 1:

solution:
1. backtracking(weights) + memo
2. k ranges iter dp
*
```

Solution Greedy:

```cpp
using ll=long long;

class Solution {
public:
    long long putMarbles(vector<int>& weights, int k) {
        //build boards
        vector<int> boards;
        for(int i=0; i<weights.size()-1; i++){
            boards.push_back(weights[i]+weights[i+1]);
        }
        //calcu maxScore - pq or sort
        sort(boards.begin(), boards.end());
        ll minScore=0;
        for(int i=0; i<k-1; i++){
            minScore+=boards[i];
        }
        //calcu minScore - same as above
        ll maxScore=0;
        for(int i=0; i<k-1; i++){
            maxScore+=boards[boards.size()-1-i];
        }
        //return res
        return maxScore-minScore;
    }
};

/*
[X X X] [X X X X] [X X X X X X] [X X X X] [X X X]

gready intuition:
we can see 1st ele and last ele always get used to calcu the score
this questions is find k-1 board in the arr to split it into k pieces
score = 1stEle+lastEle+boards, each board = ele on the left+ele on the right
*/
```