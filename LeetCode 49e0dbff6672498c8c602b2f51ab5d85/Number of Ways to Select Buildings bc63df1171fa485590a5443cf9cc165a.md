# Number of Ways to Select Buildings

#: 2222
Difficult: Medium
Tags: DP, Prefix, string
link: https://leetcode.com/problems/number-of-ways-to-select-buildings/
Priority: Medium
Status: No Idea At First
Created time: November 16, 2023 1:16 AM
Last edited time: November 17, 2023 1:26 AM
practicedTimes: 1
source: AmazonFreq

You are given a **0-indexed** binary string `s` which represents the types of buildings along a street where:

- `s[i] = '0'` denotes that the `ith` building is an office and
- `s[i] = '1'` denotes that the `ith` building is a restaurant.

As a city official, you would like to **select** 3 buildings for random inspection. However, to ensure variety, **no two consecutive** buildings out of the **selected** buildings can be of the same type.

- For example, given `s = "0**0**1**1**0**1**"`, we cannot select the `1st`, `3rd`, and `5th` buildings as that would form `"0**11**"` which is **not** allowed due to having two consecutive buildings of the same type.

Return *the **number of valid ways** to select 3 buildings.*

**Example 1:**

```
Input: s = "001101"
Output: 6
Explanation:
The following sets of indices selected are valid:
- [0,2,4] from "001101" forms "010"
- [0,3,4] from "001101" forms "010"
- [1,2,4] from "001101" forms "010"
- [1,3,4] from "001101" forms "010"
- [2,4,5] from "001101" forms "101"
- [3,4,5] from "001101" forms "101"
No other selection is valid. Thus, there are 6 total ways.

```

**Example 2:**

```
Input: s = "11100"
Output: 0
Explanation: It can be shown that there are no valid selections.

```

**Constraints:**

- `3 <= s.length <= 105`
- `s[i]` is either `'0'` or `'1'`.

```cpp
using ll=long long;

class Solution {
public:
    long long numberOfWays(string s) {
        int n=s.size();
        //for each 0, check how many 1s on left, right
        vector<int> prefix(n,0), suffix(n,0);
        for(int i=0; i<n; i++){
            prefix[i] = (i-1>=0?prefix[i-1]:0) + (s[i]=='1');
        }
        for(int i=n-1;i>=0;i--){
            suffix[i] = (i+1<n?suffix[i+1]:0) + (s[i]=='1');
        }
        ll cand1=0;
        for(int i=1; i<n-1; i++){
            if(s[i]=='0'){
                cand1+=(ll)prefix[i-1] * suffix[i+1];
            }
        }
        //for each 1, check how many 0s on left, right
        prefix=vector<int>(n,0), suffix=vector<int>(n,0);
        for(int i=0; i<n; i++){
            prefix[i] = (i-1>=0?prefix[i-1]:0) + (s[i]=='0');
        }
        for(int i=n-1;i>=0;i--){
            suffix[i] = (i+1<n?suffix[i+1]:0) + (s[i]=='0');
        }
        ll cand2=0;
        for(int i=1; i<n-1; i++){
            if(s[i]=='1'){
                cand2+=(ll)prefix[i-1] * suffix[i+1];
            }
        }

        return cand1+cand2;
    }
};

/*
s[i]:
    0: office
    1: restaurant
select 3, no consecutive two are same

return: num of valid ways to select 3 buildings
================================
n: 3~1e5
================================
for each 0
    how many 1 on the left, how many 1 on the right
        prefix[i]: including cur, how many 1 in (0~i)
            prefix[i]=prefix[i-1]+ s[i]==1
        suffix[i]
vise verse for each 1
*/
```