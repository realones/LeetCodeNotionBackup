# [lint] Subarray Sum Closest

#: 139
Difficult: Medium
Tags: Array, Contribution, OrderMap, Prefix, Sort
link: https://www.lintcode.com/problem/subarray-sum-closest
Priority: High
Status: No Idea At First
Created time: August 3, 2022 1:37 PM
Last edited time: October 19, 2023 5:52 PM
practicedTimes: 1
source: jiuzhang, 算初LinkedList&Array

Given an integer array, find a subarray with sum closest to zero. Return the indexes of the first number and last number.

Example:
**Example1**

```
Input:
[-3,1,1,-3,5]
Output:
[0,2]
Explanation: [0,2], [1,3], [1,1], [2,2], [0,4]

```

Challenge
O(nlogn) time

Related Questions:
submatrix-sum,subarray-sum-ii,minimum-size-subarray-sum,subarray-sum

use map to sort

```cpp
class Solution {
public:
    /*
     * @param nums: A list of integers
     * @return: A list of integers includes the index of the first number and the index of the last number
     */
    vector<int> subarraySumClosest(vector<int> &nums) {
        int resStart=0,resEnd=0;
        int diff=INT_MAX;
        //use map to sort the PrefixSum to find the min diff
        map<int,int> mp; //value - index
        mp[0]=-1;//init
        int sum=0;
        for(int i=0;i<nums.size();i++){
            sum+=nums[i];
            //check if there are subary with sum==0
            if(mp.count(sum)){return vector<int>{mp[sum]+1,i};}
            mp[sum]=i;
        }
        //find min diff
        vector<pair<int,int>> sums(mp.begin(),mp.end());

        for(int i=1;i<sums.size();i++){
            if(sums[i].first - sums[i-1].first < diff){
                diff=sums[i].first - sums[i-1].first;
                resStart=min(sums[i].second, sums[i-1].second)+1;
                resEnd=max(sums[i].second, sums[i-1].second);
            }
        }

        return vector<int>{resStart,resEnd};
    }
};
```