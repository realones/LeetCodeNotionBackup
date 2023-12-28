# Partition to K Equal Sum Subsets

#: 698
Difficult: Medium
Tags: Array, BackTracking, Bit Manipulation, BitMask, DP
link: https://leetcode.com/problems/partition-to-k-equal-sum-subsets/description/
Priority: High
Status: Dig More, HardToImplement, More Solution
Created time: August 22, 2023 3:58 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given an integer array `nums` and an integer `k`, return `true` if it is possible to divide this array into `k` non-empty subsets whose sums are all equal.

**Example 1:**

```
Input: nums = [4,3,2,3,5,2,1], k = 4
Output: true
Explanation: It is possible to divide it into 4 subsets (5), (1, 4), (2,3), (2,3) with equal sums.

```

**Example 2:**

```
Input: nums = [1,2,3,4], k = 3
Output: false

```

**Constraints:**

- `1 <= k <= nums.length <= 16`
- `1 <= nums[i] <= 104`
- The frequency of each element is in the range `[1, 4]`.

comment: 

TLE…., with bit_mask DP MLE, using mmmp instead of vvv to AC, added wisdompeak branch cutting to improve.

todo:

wisdomPeak another solution

Solution wisdompeak:

```cpp

```

Solution no DP - TLE:

```cpp
class Solution {
public:
    int origin_sum;

    bool canPartitionKSubsets(vector<int>& nums, int k) {
        int total=accumulate(nums.begin(), nums.end(), 0);
        if(total%k!=0) return false;
        int sum=total/k;
        origin_sum=sum;

        vector<int> visited(nums.size(),0);
        return combinationSum(nums,visited,sum,k);
    }

    bool combinationSum(vector<int>& nums, vector<int>& visited, int sum, int cnt){
        if(cnt==0) return true;
        if(sum==0) return combinationSum(nums,visited,origin_sum,cnt-1);
        if(sum<0) return false;
        
        for(int i=0; i<nums.size(); i++){
            if(visited[i]) continue;
            visited[i]=1;
            if(combinationSum(nums, visited, sum-nums[i],cnt)) return true;
            visited[i]=0;
        }
        return false;
    }
};
```

Solution: improve with DP - MLE:

```cpp
class Solution {
public:
    int origin_sum;
    vector<vector<vector<int>>> dp;

    bool canPartitionKSubsets(vector<int>& nums, int k) {
        int n=nums.size();
        int total=accumulate(nums.begin(), nums.end(), 0);
        if(total%k!=0) return false;
        int sum=total/k;
        dp.resize(1<<n,vector<vector<int>>(sum+1,vector<int>(k+1,-1)));
        origin_sum=sum;

        int state=0;
        return combinationSum(nums,state,sum,k);
    }

    bool combinationSum(vector<int>& nums, int& state, int sum, int cnt){
        if(cnt==0) return true;
        if(sum==0) {
            dp[state][sum][cnt]=combinationSum(nums,state,origin_sum,cnt-1);
            return dp[state][sum][cnt];
        }
        if(sum<0) return false;
        if(dp[state][sum][cnt]!=-1) return dp[state][sum][cnt];
        
        dp[state][sum][cnt]=false;
        for(int i=0; i<nums.size(); i++){
            if(state & (1<<i)) continue;
            state += (1<<i);
            if(combinationSum(nums, state, sum-nums[i],cnt)){
                dp[state][sum][cnt]=true;
                break;
            }
            state -= (1<<i);
        }
        return dp[state][sum][cnt];
    }
};
```

Solution: change above vvv→mpmpmp, passed

```cpp
class Solution {
public:
    int origin_sum;
    unordered_map<int,unordered_map<int,unordered_map<int,int>>> dp;

    bool canPartitionKSubsets(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end(),greater<>());//cut branch earlier
        int n=nums.size();
        int total=accumulate(nums.begin(), nums.end(), 0);
        if(total%k!=0) return false;
        int sum=total/k;
        origin_sum=sum;

        int state=0;
        return combinationSum(nums,state,sum,k);
    }

    bool combinationSum(vector<int>& nums, int& state, int sum, int cnt){
        if(cnt==0) return true;
        if(sum==0) {
            dp[state][sum][cnt]=combinationSum(nums,state,origin_sum,cnt-1);
            return dp[state][sum][cnt];
        }
        if(sum<0) return false;
        if(dp[state][sum].count(cnt)) return dp[state][sum][cnt];
        
        dp[state][sum][cnt]=false;
        for(int i=0; i<nums.size(); i++){
            if(state & (1<<i)) continue;
            state += (1<<i);
            if(combinationSum(nums, state, sum-nums[i],cnt)){
                dp[state][sum][cnt]=true;
                break;
            }
            state -= (1<<i);
        }
        return dp[state][sum][cnt];
    }
};
```

Solution: add wisdompeak branch cutting

```cpp
last = -1;
for(int i=0; i<nums.size(); i++){
{
    //...
    if (nums[i]==last)
        continue;
    last = nums[i];
    //...
```

```cpp
class Solution {
public:
    int origin_sum;
    unordered_map<int,unordered_map<int,unordered_map<int,int>>> dp;

    bool canPartitionKSubsets(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end(),greater<>());//cut branch earlier
        int n=nums.size();
        int total=accumulate(nums.begin(), nums.end(), 0);
        if(total%k!=0) return false;
        int sum=total/k;
        origin_sum=sum;

        int state=0;
        return combinationSum(nums,state,sum,k);
    }

    bool combinationSum(vector<int>& nums, int& state, int sum, int cnt){
        if(cnt==0) return true;
        if(sum==0) {
            dp[state][sum][cnt]=combinationSum(nums,state,origin_sum,cnt-1);
            return dp[state][sum][cnt];
        }
        if(sum<0) return false;
        if(dp[state][sum].count(cnt)) return dp[state][sum][cnt];
        
        dp[state][sum][cnt]=false;
        int last=-1;
        for(int i=0; i<nums.size(); i++){
            if(state & (1<<i)) continue;
            if(nums[i]==last) continue;
            state += (1<<i);
            if(combinationSum(nums, state, sum-nums[i],cnt)){
                dp[state][sum][cnt]=true;
                break;
            }
            last=nums[i];
            state -= (1<<i);
        }
        return dp[state][sum][cnt];
    }
};
```