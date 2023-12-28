# [lint] Continuous Subarray Sum

#: 402
Difficult: Medium
Tags: Contribution>0, Prefix
link: https://www.lintcode.com/problem/402/
Priority: Pass
Created time: September 18, 2022 6:38 PM
Last edited time: November 1, 2023 9:01 AM
practicedTimes: 1
source: jiuzhang, 算高FollowUp

Description

Given an integer array, find a continuous subarray where the sum of numbers is the biggest. Your code should return the index of the first number and the index of the last number. (If their are duplicate answer, return the minimum one in lexicographical order)

Example

**Example 1:**

```
Input: [-3, 1, 3, -3, 4]
Output: [1, 4]

```

**Example 2:**

```
Input: [0, 1, 0, 1]
Output: [0, 3]
Explanation: The minimum one in lexicographical order.

```

```cpp
class Solution {
public:
    /*
     * @param A: An integer array
     * @return: A list of integers includes the index of the first number and the index of the last number
     */
    //can use prefixSum in treemap, but have better solution.
    //contribution question
    vector<int> continuousSubarraySum(vector<int> &A) {
        if(A.empty()) return vector<int>();
        vector<int> res{0,0};
        int l=0;
        int tmpSum=0,sum=INT_MIN;
        for(int i=0;i<A.size();i++){
            //add to tmpSum
            tmpSum+=A[i];
            //update sum
            if(tmpSum>sum){
                sum=tmpSum;
                res={l,i};
            }
            //check if need to reset tmpSum
            if(tmpSum<0) {
                tmpSum=0;
                l=i+1;
            }
        }
        
        return res;
    }
};
```