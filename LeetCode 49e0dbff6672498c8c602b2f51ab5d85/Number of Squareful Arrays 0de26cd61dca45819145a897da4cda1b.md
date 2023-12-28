# Number of Squareful Arrays

#: 996
Difficult: Hard
Tags: Array, BackTracking, Bit Manipulation, BitMask, Math, Permutation, TSP/HamiltonianPath, dp-bitMask
link: https://leetcode.com/problems/number-of-squareful-arrays/
Priority: High
Status: More Solution
Created time: August 18, 2023 11:21 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Find the Shortest Superstring (Find%20the%20Shortest%20Superstring%204cb117fcf7a74e30b2186849382e5428.md)

An array is **squareful** if the sum of every pair of adjacent elements is a **perfect square**.

Given an integer array nums, return *the number of permutations of* `nums` *that are **squareful***.

Two permutations `perm1` and `perm2` are different if there is some index `i` such that `perm1[i] != perm2[i]`.

**Example 1:**

```
Input: nums = [1,17,8]
Output: 2
Explanation: [1,8,17] and [17,8,1] are the valid permutations.

```

**Example 2:**

```
Input: nums = [2,2,2]
Output: 1

```

**Constraints:**

- `1 <= nums.length <= 12`
- `0 <= nums[i] <= 109`

comment:

check backtracking

Solutions:

solution - TSP like, 
similar to “943. Find the Shortest Superstring”

permutation with last node matters → dp[state][lastVisitedNode]

```cpp
class Solution {
public:
    int numSquarefulPerms(vector<int>& nums) {
        int n=nums.size();
        int M=(1<<n);
        vector<vector<int>> dp(M,vector<int>(n,0));
        //init
        for(int i=0; i<n; i++){
            dp[1<<i][i]=1;
        }
        for(int mask=0; mask<M; mask++){
            for(int bit=0; bit<n; bit++){
                if((mask & (1<<bit)) == 0) continue;
                int preMask=mask^(1<<bit);
                if(preMask==0) continue;//safe guard
                unordered_set<int> visited;
                for(int preBit=0;preBit<n;preBit++){
                    if((preMask & (1<<preBit)) == 0) continue;
                    if(!isSquare(nums[preBit]+nums[bit])) continue;
                    if(visited.count(nums[preBit])) continue;
                    visited.insert(nums[preBit]);
                    dp[mask][bit] += dp[preMask][preBit];
                }
            }
        }

        int cnt=0;
        unordered_set<int> visited;
        for(int bit=0; bit<n; bit++){
            if(visited.count(nums[bit])) continue;
            visited.insert(nums[bit]);
            cnt+=dp.back()[bit];
        }
        return cnt;
    }

    bool isSquare(int a){
        return pow((int)sqrt(a),2)==a;
    }
};

/*
can have dup elements

dp[state][bit]
    state: 0000~1111, means the state of selected elements
    bit: last element selected,

dp[state][bit] +=
    dp[pre_state][pre_bit] only if (pre_bit -> bit) is square

init:
    dp[node][node] = 1

return
    sum of dp[11111][any], if bit same, treat as one bit

*/
```

Solution 2 - backtracking

```cpp

```