# Successful Pairs of Spells and Potions

#: 2300
Difficult: Medium
Tags: 2pt, Array, BS_lower_upper_bound, Sort
link: https://leetcode.com/problems/successful-pairs-of-spells-and-potions
Priority: Medium
Status: HardToImplement, More Solution
Created time: July 14, 2023 4:19 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_75

You are given two positive integer arrays `spells` and `potions`, of length `n` and `m` respectively, where `spells[i]` represents the strength of the `ith` spell and `potions[j]` represents the strength of the `jth` potion.

You are also given an integer `success`. A spell and potion pair is considered **successful** if the **product** of their strengths is **at least** `success`.

Return *an integer array* `pairs` *of length* `n` *where* `pairs[i]` *is the number of **potions** that will form a successful pair with the* `ith` *spell.*

**Example 1:**

```
Input: spells = [5,1,3], potions = [1,2,3,4,5], success = 7
Output: [4,0,3]
Explanation:
- 0th spell: 5 * [1,2,3,4,5] = [5,10,15,20,25]. 4 pairs are successful.
- 1st spell: 1 * [1,2,3,4,5] = [1,2,3,4,5]. 0 pairs are successful.
- 2nd spell: 3 * [1,2,3,4,5] = [3,6,9,12,15]. 3 pairs are successful.
Thus, [4,0,3] is returned.

```

**Example 2:**

```
Input: spells = [3,1,2], potions = [8,5,8], success = 16
Output: [2,0,2]
Explanation:
- 0th spell: 3 * [8,5,8] = [24,15,24]. 2 pairs are successful.
- 1st spell: 1 * [8,5,8] = [8,5,8]. 0 pairs are successful.
- 2nd spell: 2 * [8,5,8] = [16,10,16]. 2 pairs are successful.
Thus, [2,0,2] is returned.

```

**Constraints:**

- `n == spells.length`
- `m == potions.length`
- `1 <= n, m <= 105`
- `1 <= spells[i], potions[i] <= 105`
- `1 <= success <= 1010`

todo：2pt tag

Solution: upper_bound, 有点晕做的，两个array搞不清谁处理谁，脑子堆栈不够，要条理的记录

```cpp
using ll=long long;

class Solution {
public:
    vector<int> successfulPairs(vector<int>& spells, vector<int>& potions, long long success) {//success: 1~1e10
        int n=spells.size();//len:1~1e5, val:1~1e5
        int m=potions.size();//len:1~1e5, val:1~1e5
        //get leastSpellPowers for each potions: O(n)
        vector<double> leastPowers(potions.begin(),potions.end());
        for(auto& l: leastPowers){
            l=(double)success/l;
        }
        //sort leastSpellPowers: O(nlogn)
        sort(leastPowers.begin(), leastPowers.end());
        //for each existing spells, check how many in leastSpellPowers <= cur O(nlogn)
        vector<int> res;
        for(auto sp: spells){
            res.push_back(upper_bound(leastPowers.begin(), leastPowers.end(), sp) - leastPowers.begin());
        }
        return res;
    }
};

//s*p>=success -> successfulPairs
//return pairs(n) {num of poitions for a successfulpair for ith spell
```