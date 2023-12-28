# The Number of Beautiful Subsets

#: 2597
Difficult: Medium
Tags: Array, BackTracking, DP
link: https://leetcode.com/problems/the-number-of-beautiful-subsets/description/
Priority: High
Status: HardToImplement, More Solution, No Idea At First, toRevisit, toSummarize
Created time: December 8, 2023 7:08 PM
Last edited time: December 8, 2023 10:40 PM
source: facebookFreq

You are given an array `nums` of positive integers and a **positive** integer `k`.

A subset of `nums` is **beautiful** if it does not contain two integers with an absolute difference equal to `k`.

Return *the number of **non-empty beautiful** subsets of the array* `nums`.

A **subset** of `nums` is an array that can be obtained by deleting some (possibly none) elements from `nums`. Two subsets are different if and only if the chosen indices to delete are different.

**Example 1:**

```
Input: nums = [2,4,6], k = 2
Output: 4
Explanation: The beautiful subsets of the array nums are: [2], [4], [6], [2, 6].
It can be proved that there are only 4 beautiful subsets in the array [2,4,6].

```

**Example 2:**

```
Input: nums = [1], k = 1
Output: 1
Explanation: The beautiful subset of the array nums is [1].
It can be proved that there is only 1 beautiful subset in the array [1].

```

**Constraints:**

- `1 <= nums.length <= 20`
- `1 <= nums[i], k <= 1000`

comment: backtracking is easy, but not able to figure out the follow up

Solution lee215:

```cpp
//lee215
class Solution {
public:
    int beautifulSubsets(vector<int>& nums, int k) {
        //key val sorted in each mod group
        unordered_map<int, map<int,int>> mp;// mod-[val-freq]
        for(auto x: nums){
            mp[x%k][x]++;
        }
        int res=1;
        for(auto [m,valFreqs]: mp){
            //for each group of (val-freq) in val%k
            //dp:
            //  nVal: ways not with val
            //  wVal: ways with val
            int prev=INT_MIN/2, nVal=1, wVal=0;
            for(auto [val, freq]: valFreqs){
                int cnt=pow(2,freq)-1;//-1 to reomve the empty subset
                if(val-prev==k){
                    int newNVal = nVal+wVal;
                    int newWVal = nVal*cnt;
                    nVal=newNVal;
                    wVal=newWVal;
                }
                else{
                    int newNVal = nVal+wVal;
                    int newWVal = (nVal+wVal)*cnt;
                    nVal=newNVal;
                    wVal=newWVal;
                }
                prev=val;
            }
            res *= (nVal + wVal);
        }
        return res-1;
    }
};

/*
nums:
    len:1~20
    val: 1~1000
k: 1~1000
================================
valid: doesn't contain two interger a,b that |a-b|==k
return: num of no-empty valid subsets
================================
solution 1: 
    backtracking to find all possible solutions, filter out invalid ones
        T(2^n), n<=25
solution 2: lee215
    divided vals into groups by mod k, e.g. k=3
    g1: 1
    g2: 2 5 11
    g3: 3 6 9
    res=g1*g2*g3
for each group solution:
    stateMachine dp:
        state:
            w: using cur val
            n: not using cur val
    if continous two elems a,b that (b-a)==k
        n<= n+w
        w<= n*cnt
    else
        n<= n+w
        w<= (n+w)*cnt
    here cnt=2^freq - 1 //-1 to remove empty subset

    T: O(nlogn+k)
    S: O(n+k)
*/
```