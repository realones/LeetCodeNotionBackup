# Maximum Number of Alloys

#: 2861
Difficult: Medium
Tags: BS_Val
link: https://leetcode.com/problems/maximum-number-of-alloys/description/
Priority: High
Status: No Idea At First, checkBetterSolution
Created time: September 16, 2023 10:43 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are the owner of a company that creates alloys using various types of metals. There are `n` different types of metals available, and you have access to `k` machines that can be used to create alloys. Each machine requires a specific amount of each metal type to create an alloy.

For the `ith` machine to create an alloy, it needs `composition[i][j]` units of metal of type `j`. Initially, you have `stock[i]` units of metal type `i`, and purchasing one unit of metal type `i` costs `cost[i]` coins.

Given integers `n`, `k`, `budget`, a **1-indexed** 2D array `composition`, and **1-indexed** arrays `stock` and `cost`, your goal is to **maximize** the number of alloys the company can create while staying within the budget of `budget` coins.

**All alloys must be created with the same machine.**

Return *the maximum number of alloys that the company can create*.

**Example 1:**

```
Input: n = 3, k = 2, budget = 15, composition = [[1,1,1],[1,1,10]], stock = [0,0,0], cost = [1,2,3]
Output: 2
Explanation: It is optimal to use the 1st machine to create alloys.
To create 2 alloys we need to buy the:
- 2 units of metal of the 1st type.
- 2 units of metal of the 2nd type.
- 2 units of metal of the 3rd type.
In total, we need 2 * 1 + 2 * 2 + 2 * 3 = 12 coins, which is smaller than or equal to budget = 15.
Notice that we have 0 units of metal of each type and we have to buy all the required units of metal.
It can be proven that we can create at most 2 alloys.

```

**Example 2:**

```
Input: n = 3, k = 2, budget = 15, composition = [[1,1,1],[1,1,10]], stock = [0,0,100], cost = [1,2,3]
Output: 5
Explanation: It is optimal to use the 2nd machine to create alloys.
To create 5 alloys we need to buy:
- 5 units of metal of the 1st type.
- 5 units of metal of the 2nd type.
- 0 units of metal of the 3rd type.
In total, we need 5 * 1 + 5 * 2 + 0 * 3 = 15 coins, which is smaller than or equal to budget = 15.
It can be proven that we can create at most 5 alloys.

```

**Example 3:**

```
Input: n = 2, k = 3, budget = 10, composition = [[2,1],[1,2],[1,1]], stock = [1,1], cost = [5,5]
Output: 2
Explanation: It is optimal to use the 3rd machine to create alloys.
To create 2 alloys we need to buy the:
- 1 unit of metal of the 1st type.
- 1 unit of metal of the 2nd type.
In total, we need 1 * 5 + 1 * 5 = 10 coins, which is smaller than or equal to budget = 10.
It can be proven that we can create at most 2 alloys.

```

**Constraints:**

- `1 <= n, k <= 100`
- `0 <= budget <= 108`
- `composition.length == k`
- `composition[i].length == n`
- `1 <= composition[i][j] <= 100`
- `stock.length == cost.length == n`
- `0 <= stock[i] <= 108`
- `1 <= cost[i] <= 100`

comment:

"All alloys must be created with the same machine." i missed this one, I read it but forgot during the contest, fuck

check better solution since mine performance is not good.

```cpp
class Solution {
public:
    //n types of metals
    //k machines that can be used to create alloys
    //For the ith machine to create an alloy, it needs composition[i][j] units of metal of type j
    //Initially, stock[i] units of metal type i
    //purchasing one unit of metal type i costs cost[i] coins.
    //================================
    //return: maximize the number of alloys the company can create
    int maxNumberOfAlloys(int n, int K, int budget, vector<vector<int>>& composition, vector<int>& stock, vector<int>& cost) {
        int res=0;
        for(int k=0;k<K;k++){
            res=max(res, helper(n, budget, composition[k], stock, cost));
        }
        return res;
    }
    //YYYYXXXX
    int helper(int n, int budget, vector<int>& composition, vector<int>& stock, vector<int>& cost) {
        function valid = [&](int alloyCnt)->bool{
            long long expectedBudget=budget, acutalBudget=0;
            //for each metal, acutualBudget(init:0) += (alloy_cnt * metal_req - stock) * cost
            for(int i=0; i<n; i++){
                acutalBudget+= max(0LL,((long long)alloyCnt*composition[i]-stock[i]) * cost[i]);
            }
            return acutalBudget<=expectedBudget;
        };

        int l=0,r=1e8+1e8;//budget+stock
        while(l<r){
            int m=l+((long long)r-l+1)/2;
            if(!valid(m)) {r=m-1;}//if not qualify
            else {l=m;}
        }
        return r;
    }
};

/*
1 <= n, k <= 100
0 <= budget <= 1e8
1 <= composition[i][j] <= 100
0 <= stock[i] <= 1e8
1 <= cost[i] <= 100
================================
"All alloys must be created with the same machine." i missed this one, I read it but forgot during the contest, fuck
================================
k=100 * (max of)
    solution for each machine
        max alloys cur machine can build:
                           A    B    C
            budget
            cost.          c0,  c1,  c2
            stock.         s0,  s1,  s2 
            composition:   r0,  r1,  r2
        bs_val on: 0 ~ 1e8 -> log -> ~32
            check expectedBudget(=budget) vs acutalBudget - O(n)=O(100)
                for each metal, acutualBudget(init:0) += (alloy_cnt * metal_req - stock) * cost
                true if actual <= expected
==>
T: 100 * 32 * 100
*/
```