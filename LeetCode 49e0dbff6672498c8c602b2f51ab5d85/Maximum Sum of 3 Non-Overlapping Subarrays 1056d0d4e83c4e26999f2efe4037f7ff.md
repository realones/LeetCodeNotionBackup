# Maximum Sum of 3 Non-Overlapping Subarrays

#: 689
Difficult: Hard
Tags: Array, Prefix, dp-K_Ranges, three-pass
link: https://leetcode.com/problems/maximum-sum-of-3-non-overlapping-subarrays/description/
Priority: High
Status: HardToImplement, More Solution
Created time: August 19, 2023 4:24 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

Given an integer array `nums` and an integer `k`, find three non-overlapping subarrays of length `k` with maximum sum and return them.

Return the result as a list of indices representing the starting position of each interval (**0-indexed**). If there are multiple answers, return the lexicographically smallest one.

**Example 1:**

```
Input: nums = [1,2,1,2,6,7,5,1], k = 2
Output: [0,3,5]
Explanation: Subarrays [1, 2], [2, 6], [7, 5] correspond to the starting indices [0, 3, 5].
We could have also taken [2, 1], but an answer of [1, 3, 5] would be lexicographically larger.

```

**Example 2:**

```
Input: nums = [1,2,1,2,1,2,1,2,1], k = 2
Output: [0,2,4]

```

**Constraints:**

- `1 <= nums.length <= 2 * 104`
- `1 <= nums[i] < 216`
- `1 <= k <= floor(nums.length / 3)`

comment:

一开始想用dp_kranges，但是不熟练，看了wisdompeak后用的three pass

Solution three-pass:

```cpp
using ii=pair<int,int>;

class Solution {
public:
    vector<int> maxSumOfThreeSubarrays(vector<int>& nums, int k) {
        int n=nums.size();
        if(n<3) return {};
        vector<int> prefixSum(nums);
        for(int i=1;i<n;i++){
            prefixSum[i]+=prefixSum[i-1];
        }

        vector<ii> leftMax(n);//val_idx
        int leftMaxSum=0, leftMaxSumIdx=0;
        for(int l=0,r; r=l+k-1,r<n; l++){
            int sum=getPrefixSum(prefixSum,l,r);
            if(sum>leftMaxSum){
                leftMaxSum=sum;
                leftMaxSumIdx=l;
            }
            leftMax[l]={leftMaxSum,leftMaxSumIdx};
        }
        
        vector<ii> rightMax(n);//val_idx
        int rightMaxSum=0, rightMaxSumIdx=0;
        for(int r=n-1,l; l=r-k+1,l>=0; r--){
            int sum=getPrefixSum(prefixSum,l,r);
            if(sum>=rightMaxSum){
                rightMaxSum=sum;
                rightMaxSumIdx=l;
            }
            rightMax[l]={rightMaxSum,rightMaxSumIdx};
        }

        //iterator mid
        vector<int> res;
        int sum=0;
        for(int l=k,r; r=l+k-1,r<n-k; l++){
            int mid=getPrefixSum(prefixSum,l,r);
            int left=leftMax[l-k].first;
            int right=rightMax[r+1].first;
            int leftIdx=leftMax[l-k].second;
            int rightIdx=rightMax[r+1].second;
            if(mid+left+right>sum){
                res={leftIdx,l,rightIdx};
                sum=mid+left+right;
            }
        }
        return res;
    }

    int getPrefixSum(vector<int>& prefixSum, int l, int r){
        return prefixSum[r] - (l==0?0:prefixSum[l-1]);
    }
};

/*
from wisdom peak:
can use dp to find max ranges sum, but complex to find the idx for max ranges

three-pass:
mid: sliding window with size=k: [i:i+k-1], slide from leftMost to rightMost O(n-2k)
    O(1) to calcu sum[i:i+k-1] with prefix sum
left: [0:i-1]
    maintain leftMax[i] - maxSumRange in range 0...i
        can find by sliding a window from left to right, i:[0,n-k]
        maintain maxVal - leftMostIdx => when >pre Max, update idx
right: [i+k:n-1]
    maintain rightMax[i] - maxSumRange in range n-1...i
        can find by sliding a window from right to left, i:[n-k,0]
        maintain maxVal - leftMostIdx => when >=pre Max, update idx

res max= leftMax[i-1]+sum[i:i+k-1]+rightMax[i+k], i=[k:n-2k]
*/
```