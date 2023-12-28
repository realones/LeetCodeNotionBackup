# [lint] Subarray Sum II

#: 404
Difficult: Hard
Tags: 2pt_SameDir_diffSpeed, Prefix
link: http://www.lintcode.com/en/problem/subarray-sum-ii/
Priority: Medium
Created time: September 18, 2022 6:19 PM
Last edited time: November 1, 2023 9:56 AM
practicedTimes: 1
source: jiuzhang, 算高FollowUp
related: Subarrays with K different Integers (Subarrays%20with%20K%20different%20Integers%203a4ef2e626354a45b0f55710f665c3c3.md)

Given an positive integer array `A` and an interval. Return the number of subarrays whose sum is in the range of given interval.

Example:
**Example 1:**

```
Input: A = [1, 2, 3, 4], start = 1, end = 3
Output: 4
Explanation: All possible subarrays: [1](sum = 1), [1, 2](sum = 3), [2](sum = 2), [3](sum = 3).

```

**Example 2:**

```
Input: A = [1, 2, 3, 4], start = 1, end = 100
Output: 10
Explanation: Any subarray is ok.

```

一开始想 双指针向前移动维持一个valid window，
发现有问题，因为valid window是维护最小window或者最大window，例如[1,2,3] is valid, and [2] is also valid，这就不好办了。
所以要变形，用start来维护最小window，用end来维护最大window，要用三个指针, l, r_min, r_max
T: O(n)

```cpp
class Solution {
public:
    /**
     * @param A: An integer array
     * @param start: An integer
     * @param end: An integer
     * @return: the number of possible answer
     */
    int subarraySumII(vector<int> &nums, int start, int end) {
        // write your code here
        int n=nums.size(); if(n==0) {return 0;}
        vector<int> prefixSum(nums);
        for(int i=1;i<n;i++){
            prefixSum[i]+=prefixSum[i-1];
        }
        //window, two r
        //l r1 r2
        int res=0;
        //for l 0..n-1
        for(int l=0,r1=0,r2=0;l<n;l++){
            //  r1 find first valid
            if(r1<l){
                r1=l;
            }
            while(!valid(prefixSum,start,end,l,r1)){
                r1++;
                if(r1==n){return res;}
            }
            //  r2 find last valid
            if(r2<r1){
                r2=r1;
            }
            while(valid(prefixSum,start,end,l,r2)){
                r2++;
                if(r2==n){break;}
            }
            r2--;
            res+=r2-r1+1;
        }
        return res;
    }

    bool valid(vector<int>& prefixSum, int start,int end,int l,int r){
        return (getPrefixSum(prefixSum,l,r)>=start) && (getPrefixSum(prefixSum,l,r)<=end);
    }

    int getPrefixSum(vector<int>& prefixSum, int l, int r){
        return prefixSum[r] - (l==0?0:prefixSum[l-1]);
    }
};
```

possible better solution? question locked so not able to verify:

```cpp
class Solution {
public:
    int subarraySumII(vector<int>& nums, int lower, int upper) {
        int n=nums.size(); if(n==0) {return 0;}
        
        vector<int> prefixSum(nums);
        for(int i=1;i<n;i++){
            prefixSum[i]+=prefixSum[i-1];
        }
        
        int res=0;
        for(int i=0,r1=0,r2=0;i<n;i++){
            //find first valid l
            r1=max(l,r1);
            while(r1<n && getPrefixSum(prefixSum,i,r1)<lower) r1++;
            if(r1==n) break; //all smaller than lower
            //find last valid r
            r2=max(r1,r2);
            while(r2<n && getPrefixSum(prefixSum,i,r2)<=upper) r2++;
            r2--;
            res+=r2-r2+1;
        }
        return res;
    }
    
    int getPrefixSum(vector<int>& prefixSum, int l, int r){
        return prefixSum[r] - (l==0?0:prefixSum[l-1]);
    }
};
```