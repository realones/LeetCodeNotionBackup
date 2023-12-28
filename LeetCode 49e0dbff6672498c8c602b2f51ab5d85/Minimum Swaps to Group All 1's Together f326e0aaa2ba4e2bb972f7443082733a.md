# Minimum Swaps to Group All 1's Together

#: 1151
Difficult: Medium
Tags: Array, Sliding Window
link: https://leetcode.com/problems/minimum-swaps-to-group-all-1s-together/
Priority: Medium
Status: No Idea At First
Created time: November 13, 2023 1:46 AM
Last edited time: November 17, 2023 1:26 AM
practicedTimes: 1
source: AmazonFreq

Given a binary array `data`, return the minimum number of swaps required to group all `1`’s present in the array together in **any place** in the array.

**Example 1:**

```
Input: data = [1,0,1,0,1]
Output: 1
Explanation: There are 3 ways to group all 1's together:
[1,1,1,0,0] using 1 swap.
[0,1,1,1,0] using 2 swaps.
[0,0,1,1,1] using 1 swap.
The minimum is 1.

```

**Example 2:**

```
Input: data = [0,0,0,1,0]
Output: 0
Explanation: Since there is only one 1 in the array, no swaps are needed.

```

**Example 3:**

```
Input: data = [1,0,1,0,1,0,0,1,1,0,1]
Output: 3
Explanation: One possible solution that uses 3 swaps is [0,0,0,0,0,1,1,1,1,1,1].

```

**Constraints:**

- `1 <= data.length <= 105`
- `data[i]` is either `0` or `1`.

```cpp
class Solution {
public:
    int minSwaps(vector<int>& nums) {
        int n=nums.size();//len: 1~1e5
        int k=0;//cnt of 1, windowSz
        for(auto x: nums) if(x==1) k++;

        int minCnt=INT_MAX;
        int cnt=0;
        for(int i=0; i<n; i++){
            if(nums[i]==0) cnt++;
            if(i>=k){
                if(nums[i-k]==0) cnt--;
            }
            if(i>=k-1)
                minCnt=min(minCnt,cnt);
        }
        return minCnt;
    }
};

/*
together k ones
maintain a slidingWin with sz=k
find the window with min cnt of 0, which is the res
*/
```