# Apply Operations to Maximize Frequency Score

#: 2968
Difficult: Hard
Tags: Sliding Window, Sort, median
link: https://leetcode.com/problems/minimum-cost-to-make-array-equalindromic/description/
Priority: High
Status: HardToImplement
Created time: December 16, 2023 9:48 PM
Last edited time: December 16, 2023 10:53 PM
source: LC_Contests
related: Best Meeting Point (Best%20Meeting%20Point%208a7154960eab4a9bae8b86d36a9ffb7d.md), Minimum Cost to Make Array Equalindromic (Minimum%20Cost%20to%20Make%20Array%20Equalindromic%200d794d6bb11644538cb2293d38e6dc64.md)

You are given a **0-indexed** integer array `nums` and an integer `k`.

You can perform the following operation on the array **at most** `k` times:

- Choose any index `i` from the array and **increase** or **decrease** `nums[i]` by `1`.

The score of the final array is the **frequency** of the most frequent element in the array.

Return *the **maximum** score you can achieve*.

The frequency of an element is the number of occurences of that element in the array.

**Example 1:**

```
Input: nums = [1,2,6,4], k = 3
Output: 3
Explanation: We can do the following operations on the array:
- Choose i = 0, and increase the value of nums[0] by 1. The resulting array is [2,2,6,4].
- Choose i = 3, and decrease the value of nums[3] by 1. The resulting array is [2,2,6,3].
- Choose i = 3, and decrease the value of nums[3] by 1. The resulting array is [2,2,6,2].
The element 2 is the most frequent in the final array so our score is 3.
It can be shown that we cannot achieve a better score.

```

**Example 2:**

```
Input: nums = [1,4,4,2,4], k = 0
Output: 3
Explanation: We cannot apply any operations so our score will be the frequency of the most frequent element in the original array, which is 3.

```

**Constraints:**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 109`
- `0 <= k <= 1014`

Solution: improved from following TLE one

```cpp
using ll=long long;

class Solution {
public:
    int maxFrequencyScore(vector<int>& nums, long long k) {
        int n=nums.size();// if(n==0) {return 0;}
        sort(nums.begin(), nums.end());
        int res=0;
        ll sum=0;

        vector<ll> prefixSum(nums.begin(), nums.end());
        for(int i=1;i<n;i++){
            prefixSum[i]+=prefixSum[i-1];
        }
        function getPrefixSum = [&](int l, int r){return prefixSum[r] - (l==0?0:prefixSum[l-1]);};
        
        auto minTotalDiffsToFlat=[&](int l, int r)->ll{
            int medianIdx=l+(r-l)/2;
            ll median=nums[medianIdx];
            ll total=0;
            //left: l...median-1
            int leftCnt=medianIdx-l;
            if(leftCnt>0){
                ll leftSum=getPrefixSum(l,medianIdx-1);
                total+= median*leftCnt - leftSum;
            }
            //right: median+1...r
            int rightCnt=r-medianIdx;
            if(rightCnt>0){
                ll rightSum=getPrefixSum(medianIdx+1,r);
                total+= rightSum - median*rightCnt;
            }
            return total;
        };
        
        for(int l=0,r=0; r<n; r++){
            //add r
            //while required>k, trim l
            while(minTotalDiffsToFlat(l,r)>k){
                l++;
            }
            //update res
            res=max(res,r-l+1);
            
        }
        return res;
    }

};

/*
op: elem ++ / --
op times: <=k

score: freq of the most freq elem

return: max score
================================
nums:
    len: 1e5
    val: 1e9
k: 1e14
================================
*/
```

TLE

```cpp
using ll=long long;

class Solution {
public:
    int maxFrequencyScore(vector<int>& nums, long long k) {
        int n=nums.size();// if(n==0) {return 0;}
        sort(nums.begin(), nums.end());
        std::cout<< "nums" <<" -- "; for (const auto& elem : nums) { cout << setw(4) << elem << " "; } std::cout<<std::endl;
        int res=0;
        ll sum=0;
        
        auto req=[&](int l, int r, ll sum)->ll{
            int median=nums[l+ (r-l)/2];
            ll expected=0;
            for(int i=l;i<=r;i++){
                expected+=labs(nums[i]-median);
            }
            return expected;
        };
        
        for(int l=0,r=0; r<n; r++){
            //add r
            sum+=nums[r];
            //while required>k, trim l
            while(req(l,r,sum)>k){
                sum-=nums[l];
                l++;
            }
            //update res
            std::cout << "l,r: " << " -- " << l<<","<<r << std::endl;
            std::cout << "sum" << " -- " << sum << std::endl;
            res=max(res,r-l+1);
            
        }
        return res;
    }
};

/*
op: elem ++ / --
op times: <=k

score: freq of the most freq elem

return: max score
================================
nums:
    len: 1e5
    val: 1e9
k: 1e14
================================
*/
```