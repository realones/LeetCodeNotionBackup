# Optimal Account Balancing

#: 465
Difficult: Hard
Tags: Array, BackTracking, Bit Manipulation, BitMask, DP
link: https://leetcode.com/problems/optimal-account-balancing/
Priority: High
Status: Dig More, More Solution, No Idea At First
Created time: June 30, 2023 5:28 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

You are given an array of transactions `transactions` where `transactions[i] = [fromi, toi, amounti]` indicates that the person with `ID = fromi` gave `amounti $` to the person with `ID = toi`.

Return *the minimum number of transactions required to settle the debt*.

**Example 1:**

```
Input: transactions = [[0,1,10],[2,0,5]]
Output: 2
Explanation:
Person #0 gave person #1 $10.
Person #2 gave person #0 $5.
Two transactions are needed. One way to settle the debt is person #1 pays person #0 and #2 $5 each.

```

**Example 2:**

```
Input: transactions = [[0,1,10],[1,0,1],[1,2,5],[2,0,5]]
Output: 1
Explanation:
Person #0 gave person #1 $10.
Person #1 gave person #0 $1.
Person #1 gave person #2 $5.
Person #2 gave person #0 $5.
Therefore, person #1 only need to give person #0 $4, and all debt is settled.

```

**Constraints:**

- `1 <= transactions.length <= 8`
- `transactions[i].length == 3`
- `0 <= fromi, toi < 12`
- `fromi != toi`
- `1 <= amounti <= 100`

Solution backtracking (each people’s balance)

no idea at first, 2 solutions from LC Editorial, first one is backtracking, second one is dp with bitmask, pretty hard and need to dig more

```cpp
class Solution {
public:
    //func: splitwise like
    //{from, to, amount}
    //len: 1~8, people: <=12
    int minTransfers(vector<vector<int>>& transactions) {
        //build balance vector
        unordered_map<int,int> mp;//id->bal
        for(auto &t: transactions){
            int from=t[0], to=t[1], amount=t[2];
            mp[from]-=amount;
            mp[to]+=amount;
        }
        vector<int> bal;
        for(auto [_,b]: mp){
            if(b!=0) bal.push_back(b);
        }
        //process
        return dfs(bal, 0);
    }

    int dfs(vector<int>& bal, int start){
        if(start==bal.size()){
            return 0;
        }

        if(bal[start]==0) return dfs(bal,start+1);

        int res=INT_MAX;
        for(int i=start+1; i<bal.size(); i++){
            if(bal[start] * bal[i] > 0) continue;//both pos or neg
            bal[i]+=bal[start];
            res=min(res, dfs(bal,start+1) + 1);
            bal[i]-=bal[start];
        }
        return res;
    }
};

/*

from LC Editorial:

intuition:

for each person, get the person's final net banlance

divide into two groups: postive balance and negtive balance

greedy: for each tran, one people can tran to other people with opposite sign of balance, even if change the sign of other people

if a group of people whose total debt sum is 0, this group can be cleared without other people

================================
Solution 1: back tracking
f(i): min num of trans to settle debts for people [i:n-1]

f(i) = (trans debt to later people:0/1) + f(i+1)
    trans debt to later people:
        balance of people
            =0: no action required
                -> 0
            <0: action to later people with debt >0 - backtracking
                add cur balance[i] to the later balance[k]
                -> 1
            >0: vice versa
                -> 1
    
    how it affect f(i+1):
        mentioned above, balance[k]+=balance[i];

return f(0)

complexity:
T: O(n!)

S: O(n)
    balance arr: n
    recursive depth: n

================================
Solution 2: DP

if a group of people whose total debt sum is 0, this group can be cleared without other people
->
greedy:
find min number of subgroups, each with total debt =0
each group can save one trans, so total trans = n - subgroupsCnt

store people in group: bitmask
mp: idx0 - idx0

f(111111): min trans required
=
todo

*/
```