# Maximum Length of Semi-Decreasing Subarrays

#: 2863
Difficult: Medium
Tags: Array, Hash, Sort
link: https://leetcode.com/problems/maximum-length-of-semi-decreasing-subarrays/description/
Priority: High
Status: No Idea At First, toSummarize
Created time: December 24, 2023 2:23 AM
Last edited time: December 24, 2023 3:12 AM
source: googleFreq

You are given an integer array `nums`.

Return *the length of the **longest semi-decreasing** subarray of* `nums`*, and* `0` *if there are no such subarrays.*

- A **subarray** is a contiguous non-empty sequence of elements within an array.
- A non-empty array is **semi-decreasing** if its first element is **strictly greater** than its last element.

**Example 1:**

```
Input: nums = [7,6,5,4,3,2,1,6,10,11]
Output: 8
Explanation: Take the subarray [7,6,5,4,3,2,1,6].
The first element is 7 and the last one is 6 so the condition is met.
Hence, the answer would be the length of the subarray or 8.
It can be shown that there aren't any subarrays with the given condition with a length greater than 8.

```

**Example 2:**

```
Input: nums = [57,55,50,60,61,58,63,59,64,60,63]
Output: 6
Explanation: Take the subarray [61,58,63,59,64,60].
The first element is 61 and the last one is 60 so the condition is met.
Hence, the answer would be the length of the subarray or 6.
It can be shown that there aren't any subarrays with the given condition with a length greater than 6.

```

**Example 3:**

```
Input: nums = [1,2,3,4]
Output: 0
Explanation: Since there are no semi-decreasing subarrays in the given array, the answer is 0.

```

**Constraints:**

- `1 <= nums.length <= 105`
- `109 <= nums[i] <= 109`

toSummarize:

NLE vs smallestLE vs lastLE

```cpp
using ii=pair<int,int>;
class Solution {
public:
    int maxSubarrayLength(vector<int>& nums) {
        reverse(nums.begin(), nums.end());//i thought first<last, so save some time just reverse it..
        map<int,ii,greater<>> mp;//val - left most and right most idx
        for(int i=0;i<nums.size();i++){
            if(!mp.count(nums[i])) mp[nums[i]]={i,i};
            else mp[nums[i]].second=i;
        }

        //from large to small
        int res=0;
        int idx=-1; //rightMostIdx
        for(auto [v,p]: mp){//val,pair
            auto [l,r]=p;
            //try to reach from l to idx
            res=max(res,idx-l+1);
            idx=max(idx,r);
        }
        return res;
    }
};

/*
subarr, first>last
for each elem, find last largerThan

sort:
the 1st largest one, can't go anywhere, take it idx
the 2nd largest one, can only go to 1st largest, which is above idx, let idx=max(curIdx,idx)
the 3rd largest one, can go farthest to the idx above
...
*/
```