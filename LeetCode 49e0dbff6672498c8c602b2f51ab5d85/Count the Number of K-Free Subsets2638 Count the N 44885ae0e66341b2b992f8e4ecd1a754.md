# Count the Number of K-Free Subsets2638. Count the Number of K-Free Subsets

#: 2638
Difficult: Medium
Tags: Array, DP, Sort
link: https://leetcode.com/problems/count-the-number-of-k-free-subsets/
Priority: High
Status: No Idea At First
Created time: November 16, 2023 12:13 AM
Last edited time: November 17, 2023 1:26 AM
practicedTimes: 1
source: AmazonFreq

You are given an integer array `nums`, which contains **distinct** elements and an integer `k`.

A subset is called a **k-Free** subset if it contains **no** two elements with an absolute difference equal to `k`. Notice that the empty set is a **k-Free** subset.

Return *the number of **k-Free** subsets of* `nums`.

A **subset** of an array is a selection of elements (possibly none) of the array.

**Example 1:**

```
Input: nums = [5,4,6], k = 1
Output: 5
Explanation: There are 5 valid subsets: {}, {5}, {4}, {6} and {4, 6}.

```

**Example 2:**

```
Input: nums = [2,3,5,8], k = 5
Output: 12
Explanation: There are 12 valid subsets: {}, {2}, {3}, {5}, {8}, {2, 3}, {2, 3, 5}, {2, 5}, {2, 5, 8}, {2, 8}, {3, 5} and {5, 8}.

```

**Example 3:**

```
Input: nums = [10,5,9,11], k = 20
Output: 16
Explanation: All subsets are valid. Since the total count of subsets is 24= 16, so the answer is 16.

```

**Constraints:**

- `1 <= nums.length <= 50`
- `1 <= nums[i] <= 1000`
- `1 <= k <= 1000`

comment: 虽然可以自己做出来，但是想了很久，总想用数学方法解决，而不是dp

```cpp
using ll=long long;

class Solution {
public:
    long long countTheNumOfKFreeSubsets(vector<int>& nums, int k) {
        int kPairCnt=0, other=0;
        unordered_set<int> st(nums.begin(), nums.end());
        sort(nums.begin(), nums.end());
        vector<int> cnts;
        for(auto x: nums){
            if(!st.count(x)) continue;//already put into group
            int cnt=0;
            for(int cur=x; cur<=nums.back() && st.count(cur); cur+=k){
                cnt++;
                st.erase(cur);
            }
            cnts.push_back(cnt);
        }
        //how many subset for cnt of elems, that any pair can't selected together
        unordered_map<int,ll> dp;//cnt, numOfValidSubset
        function<ll(int)> f = [&](int cnt) {
            if(dp.count(cnt)) return dp[cnt];
            if(cnt==1) return 2LL;
            if(cnt==2) return 3LL;
            dp[cnt]=f(cnt-1)+f(cnt-2);//not_select 1st one/ select 1st one
            return dp[cnt];
        };
        //calcu
        ll res=1;
        for(auto cnt: cnts){
            //cnt-1 pairs * 2^rest
            ll tmp = f(cnt);
            res*=tmp;
        }
        return res;
    }
};
/*
subset valid if no two elems diff ==k, empty subset is valid
return: num of valid subset
================================
len: 1~50
val: 1~1000, distinct
k: 1~1000
================================
solution 1 - brute force: TLE 
    backtracking to find all subset, T:O(2^n)
        then for each one check if valid, T:O(n^2)
solution 2 - 
    maintain a list of pairs: a,b (a<b) that b-a==diff
    then when build subset,
        a,b     -> all comb - a,b
        a,b,c   -> all comb - a,b | b,c | a,b,c
        a,b,c,d -> all comb - a,b | b,c | c,d | a,b,c |...
            f(len) = 1+2+...+len
        not in pair, pick freely -> *2(pick, notpick)
*/
```