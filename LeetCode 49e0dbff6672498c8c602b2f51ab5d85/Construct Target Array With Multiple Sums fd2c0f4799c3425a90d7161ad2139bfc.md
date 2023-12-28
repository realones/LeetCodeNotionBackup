# Construct Target Array With Multiple Sums

#: 1354
Difficult: Hard
Tags: Math, PQ, simulation
link: https://leetcode.com/problems/construct-target-array-with-multiple-sums/
Priority: High
Status: Dig More, HardToImplement, No Idea At First
Created time: June 25, 2023 4:26 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

You are given an array `target` of n integers. From a starting array `arr` consisting of `n` 1's, you may perform the following procedure :

- let `x` be the sum of all elements currently in your array.
- choose index `i`, such that `0 <= i < n` and set the value of `arr` at index `i` to `x`.
- You may repeat this procedure as many times as needed.

Return `true` *if it is possible to construct the* `target` *array from* `arr`*, otherwise, return* `false`.

**Example 1:**

```
Input: target = [9,3,5]
Output: true
Explanation: Start with arr = [1, 1, 1]
[1, 1, 1], sum = 3 choose index 1
[1, 3, 1], sum = 5 choose index 2
[1, 3, 5], sum = 9 choose index 0
[9, 3, 5] Done

```

**Example 2:**

```
Input: target = [1,1,1,2]
Output: false
Explanation: Impossible to create target array from [1,1,1,1].

```

**Example 3:**

```
Input: target = [8,5]
Output: true

```

**Constraints:**

- `n == target.length`
- `1 <= n <= 5 * 104`
- `1 <= target[i] <= 109`

Solution:

思路难找，完全没想到怎么做。

用的huahua的答案，再整理一下

```cpp
using ll=long long;
class Solution {
public:
    bool isPossible(vector<int>& target) {//len: 1~5e4, val: 1~1e9
        priority_queue<int> pq(target.begin(), target.end());
        ll sum=accumulate(target.begin(),target.end(),0LL);//constructor is heapify: o(n)
        while(pq.top()>1){
            int m=pq.top(); pq.pop();
            sum-=m;//get rest sum
            if(sum==1) return true;//[1,m] case
            if(sum==0 || sum>=m) return false;//[m] case || [1,1,2] case
            m%=sum;
            if(m==0) return false;//[x,y,k*(x,y)] case
            pq.push(m);
            sum+=m;
        }
        return true;
    }
};

/*
from Huahua:
[1,1,1,1] -> target, it is a search tree, exponential increasing complexity
target -> [1,1,1,1], only one path

find the maxEle and -= the rest
e.g. 
9 3 5: 9=x+3+5
1 3 5: 5=1+3+x
1 3 1: 3=1+x+1
1 1 1: return true

8 5
3 5
3 2
1 2
1 1

1 1 1 2
1 1 1 -1

================================
complexity issue:
1 101
1 100
1 99
...
1 1
100 steps, O(maxVal)*O(log_len), TLE
================================
so need to maxElement-k*restSum, that maxElement is first one that no longer the biggest
(or less than restSum is also good enough)
maxEle = maxEle%resSum, if not change, return false

e.g. 
1 7 101
1 7 5
1 5 1
1 1 1
================================
corner case:
1 101
101%1=0
rest=1 return true

2 4
2 0 -> 4%2=0
maxEle%restSum=0 return false

2
only 1 element, if not one, return false
================================
complexity calculation:
T: O(n+klogn), k=log(n*t)
S: O(n)
each step, the sum will be at least halved, there are at most k=log(n*t)~46 steps

worst case:
1 4 7 13, sum=25
1 4 7 1, sum=13
1 4 1 1, sum=7
1 1 1 1, sum=4
sum is halved each step

best case:
1 1e9, sum=1e9+1
1 1,   sum=2
sum is directly go to very small 
*/
```

**Solution** - incorrect greedy solution:

failed case: [8,5]

```cpp
using ll=long long;

class Solution {
public:
    bool isPossible(vector<int>& nums) {//nums val: 1~1e9
        int n=nums.size();//1~5e4
        sort(nums.begin(), nums.end());
        vector<ll> arr(n,1);
        for(int i=0;i<n;i++){
            if(nums[i]==1) continue;
            ll sum=accumulate(arr.begin(),arr.end(),0);
            arr[i]=sum;
            if(arr[i]>(ll)nums[i]) return false;
            else if(arr[i]==(ll)nums[i]) continue;
            else if(arr[i]<(ll)nums[i]) i--;
        }
        return true;
    }
}
```