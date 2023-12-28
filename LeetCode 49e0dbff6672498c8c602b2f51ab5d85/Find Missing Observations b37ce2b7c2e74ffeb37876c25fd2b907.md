# Find Missing Observations

#: 2028
Difficult: Medium
Tags: Array, Math, simulation
link: https://leetcode.com/problems/find-missing-observations/description/
Priority: Low
Created time: December 16, 2023 12:16 AM
Last edited time: December 16, 2023 12:16 AM
source: microsoftFreq

You have observations of `n + m` **6-sided** dice rolls with each face numbered from `1` to `6`. `n` of the observations went missing, and you only have the observations of `m` rolls. Fortunately, you have also calculated the **average value** of the `n + m` rolls.

You are given an integer array `rolls` of length `m` where `rolls[i]` is the value of the `ith` observation. You are also given the two integers `mean` and `n`.

Return *an array of length* `n` *containing the missing observations such that the **average value** of the* `n + m` *rolls is **exactly*** `mean`. If there are multiple valid answers, return *any of them*. If no such array exists, return *an empty array*.

The **average value** of a set of `k` numbers is the sum of the numbers divided by `k`.

Note that `mean` is an integer, so the sum of the `n + m` rolls should be divisible by `n + m`.

**Example 1:**

```
Input: rolls = [3,2,4,3], mean = 4, n = 2
Output: [6,6]
Explanation: The mean of all n + m rolls is (3 + 2 + 4 + 3 + 6 + 6) / 6 = 4.

```

**Example 2:**

```
Input: rolls = [1,5,6], mean = 3, n = 4
Output: [2,3,2,2]
Explanation: The mean of all n + m rolls is (1 + 5 + 6 + 2 + 3 + 2 + 2) / 7 = 3.

```

**Example 3:**

```
Input: rolls = [1,2,3,4], mean = 6, n = 4
Output: []
Explanation: It is impossible for the mean to be 6 no matter what the 4 missing rolls are.

```

**Constraints:**

- `m == rolls.length`
- `1 <= n, m <= 105`
- `1 <= rolls[i], mean <= 6`

```cpp
class Solution {
public:
    vector<int> missingRolls(vector<int>& rolls, int mean, int n) {
        int m=rolls.size();
        int totalM = accumulate(rolls.begin(), rolls.end(),0);
        // totalN = mean * (m+n) - totalM
        int totalN = mean*(m+n) - totalM;
        
        // 1*n.~totalN~  6*n
        if(totalN<1*n || totalN>6*n) return {};

        // if valid
        // arr(n,<1~6>)
        vector<int> res;
        // init each val = floor(totalN*1.0/n),  check valid
        int val=totalN/n;
        // then the totalDiff= totalN - val*n
        int totalDiff = totalN-val*n;
        // for totalDiff num of elems, +=1
        for(int i=0;i<n-totalDiff;i++){
            res.push_back(val);
        }
        for(int i=0;i<totalDiff;i++){
            res.push_back(val+1);
        }
        return res;
    }
};

/*
n+m : rand of 1~6

n missing
m known: rolls, rolls[i]=valOfRoll
totalVal/(m+n) known: mean

return any valid arr(n,?) make the mean
fallback: empty arr

guarantee: totalVal%(n+m)==0

================================
m,n: 1~1e5
================================
(totalM + totalN) / (m+n) = mean
totalN = mean * (m+n) - totalM

arr(n,<1~6>)

init each val = floor(totalN*1.0/n),  check valid
1*n.~totalN~  6*n

if valid
then the totalDiff= totalN - val*n
for totalDiff num of elems, +=1

*/
```