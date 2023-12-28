# 3Sum With Multiplicity

#: 923
Difficult: Medium
Tags: 2pt, Array, Counting, Hash, Math, Sort
link: https://leetcode.com/problems/3sum-with-multiplicity/
Priority: Low
Status: More Solution
Created time: May 12, 2023 2:43 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, wisdompeak

Given an integer array `arr`, and an integer `target`, return the number of tuples `i, j, k` such that `i < j < k` and `arr[i] + arr[j] + arr[k] == target`.

As the answer can be very large, return it **modulo** `109 + 7`.

**Example 1:**

```
Input: arr = [1,1,2,2,3,3,4,4,5,5], target = 8
Output: 20
Explanation:
Enumerating by the values (arr[i], arr[j], arr[k]):
(1, 2, 5) occurs 8 times;
(1, 3, 4) occurs 8 times;
(2, 2, 4) occurs 2 times;
(2, 3, 3) occurs 2 times.

```

**Example 2:**

```
Input: arr = [1,1,2,2,2,2], target = 5
Output: 12
Explanation:
arr[i] = 1, arr[j] = arr[k] = 2 occurs 12 times:
We choose one 1 from [1,1] in 2 ways,
and two 2s from [2,2,2,2] in 6 ways.

```

**Example 3:**

```
Input: arr = [2,1,3], target = 6
Output: 1
Explanation: (1, 2, 3) occured one time in the array so we return 1.

```

**Constraints:**

- `3 <= arr.length <= 3000`
- `0 <= arr[i] <= 100`
- `0 <= target <= 300`

```cpp
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int threeSumMulti(vector<int>& nums, int target) {
        int n=nums.size();// if(n==0) {return 0;}
        ll res=0;
        for(int i=0;i<n;i++){
            unordered_map<int,ll> mp;//int-cnt
            target-=nums[i];
            for(int j=i+1; j<n; j++){
                if(mp.count(target-nums[j])){
                    res+=mp[target-nums[j]];
                    res%=M;
                }
                mp[nums[j]]++;
            }
            target+=nums[i];
        }
        return (res+M)%M;
    }
};
```