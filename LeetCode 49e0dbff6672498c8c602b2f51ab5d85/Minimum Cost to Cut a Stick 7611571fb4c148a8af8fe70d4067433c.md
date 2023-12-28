# Minimum Cost to Cut a Stick

#: 1547
Difficult: Hard
Tags: dp-Pyr
link: https://leetcode.com/problems/minimum-cost-to-cut-a-stick/
Priority: Medium
Created time: May 28, 2023 12:17 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

Given a wooden stick of length `n` units. The stick is labelled from `0` to `n`. For example, a stick of length **6** is labelled as follows:

![https://assets.leetcode.com/uploads/2020/07/21/statement.jpg](https://assets.leetcode.com/uploads/2020/07/21/statement.jpg)

Given an integer array `cuts` where `cuts[i]` denotes a position you should perform a cut at.

You should perform the cuts in order, you can change the order of the cuts as you wish.

The cost of one cut is the length of the stick to be cut, the total cost is the sum of costs of all cuts. When you cut a stick, it will be split into two smaller sticks (i.e. the sum of their lengths is the length of the stick before the cut). Please refer to the first example for a better explanation.

Return *the minimum total cost* of the cuts.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/07/23/e1.jpg](https://assets.leetcode.com/uploads/2020/07/23/e1.jpg)

```
Input: n = 7, cuts = [1,3,4,5]
Output: 16
Explanation: Using cuts order = [1, 3, 4, 5] as in the input leads to the following scenario:

The first cut is done to a rod of length 7 so the cost is 7. The second cut is done to a rod of length 6 (i.e. the second part of the first cut), the third is done to a rod of length 4 and the last cut is to a rod of length 3. The total cost is 7 + 6 + 4 + 3 = 20.
Rearranging the cuts to be [3, 5, 1, 4] for example will lead to a scenario with total cost = 16 (as shown in the example photo 7 + 4 + 3 + 2 = 16).
```

![https://assets.leetcode.com/uploads/2020/07/21/e11.jpg](https://assets.leetcode.com/uploads/2020/07/21/e11.jpg)

**Example 2:**

```
Input: n = 9, cuts = [5,6,1,4,2]
Output: 22
Explanation: If you try the given cuts ordering the cost will be 25.
There are much ordering with total cost <= 25, for example, the order [4, 6, 5, 2, 1] has total cost = 22 which is the minimum possible.

```

**Constraints:**

- `2 <= n <= 106`
- `1 <= cuts.length <= min(n - 1, 100)`
- `1 <= cuts[i] <= n - 1`
- All the integers in `cuts` array are **distinct**.

```cpp
class Solution {
public:
    int minCost(int stickSz, vector<int>& cuts) {
        sort(cuts.begin(), cuts.end());
        vector<int> nums;
        int preCut=0;
        for(int i=0;i<cuts.size();i++){
            nums.push_back(cuts[i]-preCut);
            preCut=cuts[i];
        }
        nums.push_back(stickSz-preCut);
        int n=nums.size();
        
        vector<int> prefixSum(nums);
        for(int i=1;i<n;i++){
            prefixSum[i]+=prefixSum[i-1];
        }
        
        vector<vector<int>> dp(n,vector<int>(n,INT_MAX));
        for(int i=0; i<n; i++){
            dp[i][i]=0;
        }
        for(int len=2;len<=n;len++){//len of a brick
            for(int l=0,r;r=l+len-1,r<n;l++){//for each brick
                //l~i,i+1~r
                for(int i=l;i+1<=r;i++){
                    dp[l][r]=min(dp[l][r],
                                    getPrefixSum(prefixSum,l,r)+dp[l][i]+dp[i+1][r]
                                );
                }
            }
        }
        return dp[0][n-1];
    }
    
    int getPrefixSum(vector<int>& prefixSum, int l, int r){
        return prefixSum[r] - (l==0?0:prefixSum[l-1]);
    }
};

/*
wood 0...n
cut  1...n-1

cost of cut: cut [i,j] = j-i, cut int(i,j)
    
    f(ij) = cost(i,j) + any cut in (i,j) that
        f(i,cut)+f(cut,j)
    
    
costij->prefixsumij    
val to final cutted len
i,j to cutted range idx
    
    
n = 7, cuts = [1,3,4,5]  
->
1,2,1,1,2
->pyr
*/
```