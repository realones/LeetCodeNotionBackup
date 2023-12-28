# Paint House II

#: 265
Difficult: Hard
Tags: dp-TimeSeqFromLastOne-Power, stateMachine
link: https://leetcode.com/problems/paint-house-ii/
Priority: Low
Created time: May 23, 2023 4:10 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily
related: Dice Roll Simulation (Dice%20Roll%20Simulation%20b60a21b3011943b6b8dec4a34dd31f4d.md)

There are a row of `n` houses, each house can be painted with one of the `k` colors. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by an `n x k` cost matrix costs.

- For example, `costs[0][0]` is the cost of painting house `0` with color `0`; `costs[1][2]` is the cost of painting house `1` with color `2`, and so on...

Return *the minimum cost to paint all houses*.

**Example 1:**

```
Input: costs = [[1,5,3],[2,9,4]]
Output: 5
Explanation:
Paint house 0 into color 0, paint house 1 into color 2. Minimum cost: 1 + 4 = 5;
Or paint house 0 into color 2, paint house 1 into color 0. Minimum cost: 3 + 2 = 5.

```

**Example 2:**

```
Input: costs = [[1,3],[2,4]]
Output: 5

```

**Constraints:**

- `costs.length == n`
- `costs[i].length == k`
- `1 <= n <= 100`
- `2 <= k <= 20`
- `1 <= costs[i][j] <= 20`

```cpp
class Solution {
public:
    int minCostII(vector<vector<int>>& costs) {
        int n=costs.size();
        int k=costs[0].size();
        
        vector<vector<int>> dp(n,vector<int>(k,INT_MAX));
        for(int i=0; i<n; i++){
            for(int j=0; j<k; j++){
                for(int t=0;t<k;t++){
                    if(t==j) continue;
                    dp[i][j]=min(dp[i][j], (i-1>=0?dp[i-1][t]:0)+costs[i][j]);
                }
            }
        }
        
        return *min_element(dp.back().begin(),dp.back().end());
    }
};
```