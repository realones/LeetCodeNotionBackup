# Maximum Average Subarray II

#: 644
Difficult: Hard
Tags: BS_Val, Prefix, Sliding Window
link: https://leetcode.com/problems/maximum-average-subarray-ii/
Priority: High
Status: Dig More, No Idea At First, toRevisit, toSummarize
Created time: September 9, 2022 10:47 AM
Last edited time: October 29, 2023 5:51 PM
practicedTimes: 1
source: jiuzhang, 算高BS&SwipeLine

You are given an integer array `nums` consisting of `n` elements, and an integer `k`.

Find a contiguous subarray whose **length is greater than or equal to** `k` that has the maximum average value and return *this value*. Any answer with a calculation error less than `10-5` will be accepted.

**Example 1:**

```
Input: nums = [1,12,-5,-6,50,3], k = 4
Output: 12.75000
Explanation:
- When the length is 4, averages are [0.5, 12.75, 10.5] and the maximum average is 12.75
- When the length is 5, averages are [10.4, 10.8] and the maximum average is 10.8
- When the length is 6, averages are [9.16667] and the maximum average is 9.16667
The maximum average is when we choose a subarray of length 4 (i.e., the sub array [12, -5, -6, 50]) which has the max average 12.75, so we return 12.75
Note that we do not consider the subarrays of length < 4.

```

**Example 2:**

```
Input: nums = [5], k = 1
Output: 5.00000

```

**Constraints:**

- `n == nums.length`
- `1 <= k <= n <= 104`
- `104 <= nums[i] <= 104`

1.暴力，用prefix sum可以优化到O(n^2)

2.想了很久，还可以从小到大推导，

- 一开始计算ave for {A[0]}, {A[1]},…{A[n-1]} ,
- then {A[0],A[1]}, {A[1],A[2]}, …,{A[n-2],A[n-1]},
- …
- until {A[1],…A[n-1]}

用之前的解，从小到大，DP，一样是O(n^2)

3.BS：看的答案，但凡是碰到数学公式推导的题目，都很难。。

```cpp
class Solution {
public:
    /*
     * @param nums: an array with positive and negative numbers
     * @param k: an integer
     * @return: the maximum average
     */
    //BS => Biggest Average that has length >= k.
    //value BS
    /*
    range nums.min~nums.max
    with eps=1e-6

    check if such ave(m), exist subarray's ave> it, l=m; else r=m
    a1+...+an>n*ave
    (a1-ave)+...+(an-ave)>0
    all subarray sum>0
    means prefixSum keep decreasing
    */
    double findMaxAverage(vector<int> &nums, int k) {
        if(nums.empty() || k<1) return 0;
        //get possible range
        int minVal=INT_MAX,maxVal=INT_MIN;
        for(auto a:nums){
            minVal=min(minVal,a);
            maxVal=max(maxVal,a);
        }
        //BS on value, find the last valid YYYYYNNNNNN, that exist subarray w/ ave>=mid
        double eps=1e-5;
        double l=minVal,r=maxVal;
        //YYYYYYYNNNNNNNNNNN
        while(r-l>eps){//T O(logk * n) k:max-min, S: can be prioritize to O(n)
            double m=l+(r-l)/2.0;
            if(isValid(nums,k,m)) l=m;
            else r=m;
        }
        return l;
    }
    /*
    (a1+a2+a3...+aj)/j≥mid =>
    (a1+a2+a3...+aj)≥j∗mid =>
    (a1−mid)+(a2−mid)+(a3−mid)...+(aj−mid)≥0
    */
    //check if exist subarray w/ ave>=mid
    bool isValid(vector<int> &nums, int k, double target){
        int n=nums.size();
        vector<double> preSum(n,0); //S O(n)
        double pre_min=0;//min val before k length elements
        double sum=0;
        for(int i=0;i<n;i++){//T O(n)
            //calcu preSum
            sum+=nums[i]-target;
            preSum[i]=sum;
            //calcu pre_min
            if(i>=k){
                pre_min=min(pre_min,preSum[i-k]);
            }
            //check res
            if(i>=k-1 && preSum[i]>=pre_min) return true;
        }
        return false;
    }
};
```