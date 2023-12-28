# Stone Game III

#: 1406
Difficult: Hard
Tags: Game, dp-TimeSeqFromLastOne-Power
link: https://leetcode.com/problems/stone-game-iii/
Priority: Medium
Created time: May 26, 2023 6:21 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, leetcode_daily

Alice and Bob continue their games with piles of stones. There are several stones **arranged in a row**, and each stone has an associated value which is an integer given in the array `stoneValue`.

Alice and Bob take turns, with Alice starting first. On each player's turn, that player can take `1`, `2`, or `3` stones from the **first** remaining stones in the row.

The score of each player is the sum of the values of the stones taken. The score of each player is `0` initially.

The objective of the game is to end with the highest score, and the winner is the player with the highest score and there could be a tie. The game continues until all the stones have been taken.

Assume Alice and Bob **play optimally**.

Return `"Alice"` *if Alice will win,* `"Bob"` *if Bob will win, or* `"Tie"` *if they will end the game with the same score*.

**Example 1:**

```
Input: values = [1,2,3,7]
Output: "Bob"
Explanation: Alice will always lose. Her best move will be to take three piles and the score become 6. Now the score of Bob is 7 and Bob wins.

```

**Example 2:**

```
Input: values = [1,2,3,-9]
Output: "Alice"
Explanation: Alice must choose all the three piles at the first move to win and leave Bob with negative score.
If Alice chooses one pile her score will be 1 and the next move Bob's score becomes 5. In the next move, Alice will take the pile with value = -9 and lose.
If Alice chooses two piles her score will be 3 and the next move Bob's score becomes 3. In the next move, Alice will take the pile with value = -9 and also lose.
Remember that both play optimally so here Alice will choose the scenario that makes her win.

```

**Example 3:**

```
Input: values = [1,2,3,6]
Output: "Tie"
Explanation: Alice cannot win this game. She can end the game in a draw if she decided to choose all the first three piles, otherwise she will lose.

```

**Constraints:**

- `1 <= stoneValue.length <= 5 * 104`
- `1000 <= stoneValue[i] <= 1000`

**Solution**:

```cpp
class Solution {
public:
    string stoneGameIII(vector<int>& nums) {
        int n=nums.size();
        vector<int> prefixSum(nums);
        for(int i=1;i<n;i++){
            prefixSum[i]+=prefixSum[i-1];
        }
        vector<int> dp(n,-5e7);
        
        for(int i=0; i<n; i++){
            dp[i]= getPrefixSum(prefixSum,n-1-i,n-1) - min({
               i-1>=0?dp[i-1]:0,
               i-2>=0?dp[i-2]:0,
               i-3>=0?dp[i-3]:0
            });
        }
        
        int sum=accumulate(nums.begin(),nums.end(),0);
        if(dp.back()*2>sum) return "Alice";
        else if(dp.back()*2==sum) return "Tie";
        else return "Bob";
    }
    
    int getPrefixSum(vector<int>& prefixSum, int l, int r){
        return prefixSum[r] - (l==0?0:prefixSum[l-1]);
    }
};

/*
v: -1000~+1000
len 5*1e4  -> dp  

return
dp all * 2 >  sum: return Alice
dp all * 2 == sum: return Tie
dp all * 2 <  sum: return Bob

dp l: l to n-1, max points
dp l max=
    1. sumln-1 - dpl+1 (if newL>n-1, dpNewL as 0)
    2. sumln-1 - dpl+2
    3. sumln-1 - dpl+3
    
dp i: i=n-1-l, i is last idx offset to left side, from 0 to n-1

for i 0..n-1   
dp i max=
    1. sum[n-1-i,n-1] - dp[n-1] (if newL>n-1, dpNewL as 0)
    2. sum[n-1-i,n-1] - dp[n-2]
    3. sum[n-1-i,n-1] - dp[n-3]
*/
```