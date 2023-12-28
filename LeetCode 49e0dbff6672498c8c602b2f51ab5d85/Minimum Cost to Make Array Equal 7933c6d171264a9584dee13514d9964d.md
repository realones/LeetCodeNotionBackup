# Minimum Cost to Make Array Equal

#: 2448
Difficult: Hard
Tags: Array, BS_todo, Greedy, Math, Prefix, Sort, Sweep-Line
link: https://leetcode.com/problems/minimum-cost-to-make-array-equal/
Priority: High
Status: HardToImplement, More Solution, No Idea At First
Created time: June 20, 2023 8:33 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

You are given two **0-indexed** arrays `nums` and `cost` consisting each of `n` **positive** integers.

You can do the following operation **any** number of times:

- Increase or decrease **any** element of the array `nums` by `1`.

The cost of doing one operation on the `ith` element is `cost[i]`.

Return *the **minimum** total cost such that all the elements of the array* `nums` *become **equal***.

**Example 1:**

```
Input: nums = [1,3,5,2], cost = [2,3,1,14]
Output: 8
Explanation: We can make all the elements equal to 2 in the following way:
- Increase the 0th element one time. The cost is 2.
- Decrease the 1st element one time. The cost is 3.
- Decrease the 2nd element three times. The cost is 1 + 1 + 1 = 3.
The total cost is 2 + 3 + 3 = 8.
It can be shown that we cannot make the array equal with a smaller cost.

```

**Example 2:**

```
Input: nums = [2,2,2,2,2], cost = [4,2,8,1,3]
Output: 0
Explanation: All the elements are already equal, so no operations are needed.

```

**Constraints:**

- `n == nums.length == cost.length`
- `1 <= n <= 105`
- `1 <= nums[i], cost[i] <= 106`

Solution 1:

```cpp
using ll=long long;

class Solution {
public:
    long long minCost(vector<int>& nums, vector<int>& cost) {
        int n=nums.size();
        vector<pair<ll,ll>> arr;
        for(int i=0; i<n; i++){arr.push_back({nums[i], cost[i]});}
        sort(arr.begin(), arr.end());

        vector<ll> left(n,0);
        ll sum=0;
        for(int i=1; i<n; i++){
            sum+=arr[i-1].second;
            left[i]= left[i-1] + (arr[i].first-arr[i-1].first) * sum;
        }
        vector<ll> right(n,0);
        sum=0;
        for(int i=n-2; i>=0; i--){
            sum+=arr[i+1].second;
            right[i]= right[i+1] + (arr[i+1].first-arr[i].first) * sum;
        }
        ll res=LLONG_MAX;
        for(int i=0; i<n; i++){
            res=min(res, left[i]+right[i]);
        }
        return res;
    }
};

/*
min: sum: abs(num[i]-x)*cost[i]

sort based on nums

solution 1: 
assume all cost is 1, 
then thinking of 1D plain, you have points nums[0],nums[1],..,nums[n-1], 
the goal is to find the point that sum of dist to all points is smallest
then it is to find the median

since cost is cost[i], can be converted to there is cost[i] number of points on position num[i];

solution 2:
Proof: the x must be on existing position
    e.g. nums: 1 2 3 5
                i j
    assume x is best solution that i<x && x<j
    totalCost=
        leftSum = sum(l:[0,i]): (num[l]-x)*cost[l]
        +
        rigntSum= sum(r:[j,n-1]): (num[r]-x)*cost[r]
    when i++
        totalCost+= delta=
            + (cost[0] + cost[1] + .. + cost[i])
            - (cost[j] + cost[j+1] + .. + cost[n-1])
    when i--
        totalCost+= delta=
            - (cost[0] + cost[1] + .. + cost[i])
            + (cost[j] + cost[j+1] + .. + cost[n-1])
    until go to i or j, find the better solution.

then use sweepLine:
    for each number
        totalCost=leftSum+rightSum;
    return min of each totalCost

    i-1 -> i
        leftSum = preLeftSum + dist(nums[i-1], nums[i]) * (cost[0]+cost[1]+...+cost[i-1])
*/
```

Solution 2:

```cpp
using ll=long long;

class Solution {
public:
    long long minCost(vector<int>& nums, vector<int>& cost) {
        int n=nums.size();
        vector<pair<ll,ll>> arr;
        for(int i=0; i<n; i++){arr.push_back({nums[i], cost[i]});}
        sort(arr.begin(), arr.end());

        ll total=accumulate(cost.begin(), cost.end(), 0LL);
        ll sum=0;
        int pivot=0;
        for(int i=0; i<n; i++){
            sum+=arr[i].second;
            if(sum>= total*0.5){
                pivot=arr[i].first;
                break;
            }
        }

        ll res=0;
        for(int i=0; i<n; i++){
            res+=abs(arr[i].first-pivot) * arr[i].second;
        }
        return res;
    }
};

/*
min: sum: abs(num[i]-x)*cost[i]

sort based on nums

solution 1: 
assume all cost is 1, 
then thinking of 1D plain, you have points nums[0],nums[1],..,nums[n-1], 
the goal is to find the point that sum of dist to all points is smallest
then it is to find the median

since cost is cost[i], can be converted to there is cost[i] number of points on position num[i];

solution 2:
Proof: the x must be on existing position
    e.g. nums: 1 2 3 5
                i j
    assume x is best solution that i<x && x<j
    totalCost=
        leftSum = sum(l:[0,i]): (num[l]-x)*cost[l]
        +
        rigntSum= sum(r:[j,n-1]): (num[r]-x)*cost[r]
    when i++
        totalCost+= delta=
            + (cost[0] + cost[1] + .. + cost[i])
            - (cost[j] + cost[j+1] + .. + cost[n-1])
    when i--
        totalCost+= delta=
            - (cost[0] + cost[1] + .. + cost[i])
            + (cost[j] + cost[j+1] + .. + cost[n-1])
    until go to i or j, find the better solution.

then use sweepLine:
    for each number
        totalCost=leftSum+rightSum;
    return min of each totalCost

    i-1 -> i
        leftSum = preLeftSum + dist(nums[i-1], nums[i]) * (cost[0]+cost[1]+...+cost[i-1])
*/
```