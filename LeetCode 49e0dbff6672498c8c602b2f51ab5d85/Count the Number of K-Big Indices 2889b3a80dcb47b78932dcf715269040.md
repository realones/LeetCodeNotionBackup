# Count the Number of K-Big Indices

#: 2519
Difficult: Hard
Tags: Array, BS_todo, BinaryIndexedTree, Divide and Conquer, MergeSort, OrderSet, PQ, SegmentTree_val
link: https://leetcode.com/problems/count-the-number-of-k-big-indices/
Priority: High
Status: Dig More, More Solution, No Idea At First, toSummarize
Created time: November 15, 2023 3:08 AM
Last edited time: November 19, 2023 3:11 PM
practicedTimes: 1
source: AmazonFreq
related: Find Building Where Alice and Bob Can Meet (Find%20Building%20Where%20Alice%20and%20Bob%20Can%20Meet%20541c5473515546d8adc8aa52d8893d9f.md)

You are given a **0-indexed** integer array `nums` and a positive integer `k`.

We call an index `i` **k-big** if the following conditions are satisfied:

- There exist at least `k` different indices `idx1` such that `idx1 < i` and `nums[idx1] < nums[i]`.
- There exist at least `k` different indices `idx2` such that `idx2 > i` and `nums[idx2] < nums[i]`.

Return *the number of k-big indices*.

**Example 1:**

```
Input: nums = [2,3,6,5,2,3], k = 2
Output: 2
Explanation: There are only two 2-big indices in nums:
- i = 2 --> There are two valid idx1: 0 and 1. There are three valid idx2: 2, 3, and 4.
- i = 3 --> There are two valid idx1: 0 and 1. There are two valid idx2: 3 and 4.

```

**Example 2:**

```
Input: nums = [1,1,1], k = 3
Output: 0
Explanation: There are no 3-big indices in nums.

```

**Constraints:**

- `1 <= nums.length <= 105`
- `1 <= nums[i], k <= nums.length`

comment:

toSummrize: 

1. find cnt of smaller ones on the left of idx - both range and val considered
2. OrderSet vs SegTree_val, seems they can cover similar questions,
3. OrderSet+k vs PQ, if only checking sz, seems very similar

todo:

for now, pq solution implemented, orderSet and segTree_val is not implemented, and there are other solutions as well

Solution:

```cpp
class Solution {
public:
    int kBigIndices(vector<int>& nums, int k) {
        int n=nums.size();//n:1~1e5
        priority_queue<int> left, right;

        vector<int> valid(n,false);
        //left
        for(int i=0; i<n; i++){
            if(left.size()==k && left.top() < nums[i]) valid[i]=true;//valid
            left.push(nums[i]);
            if(left.size()>k) left.pop();//only keep kth smallest
        }
        //right
        int res=0;
        for(int i=n-1;i>=0;i--){
            if(right.size()==k && right.top() < nums[i]) {
                if(valid[i]) res++;
            }
            right.push(nums[i]);
            if(right.size()>k) right.pop();//only keep kth smallest
        }
        return res;
    }
};

/*
len: 1~1e5
val: 1~len
k:   1~len
================================
index i is valid when both:
at least k smaller elems on the left, and k smaller elems on the right
return: cnt of valid idx
================================
for each idx, record: how many smaller on the left, how many smaller on the right
build an arr
l[i] = 
then loop the arr the get the res
================================
subquestion:
for each element:
    how many element smaller than current element
    both range(0~curIdx-1) and val(<=curVal) considered
solution 1:
    segTree by val - in order to find cnt of nodes
    for the range(curIdx-1), we gradually build the tree
        i.e. for each elem, find cur res, add cur node, 
solution 2:
    OrderSet: similar to above
================================
solution 3:
    pq:
    e.g. checking left
        maintain kth smallest on the left, if < cur, then cur is valid
*/
```