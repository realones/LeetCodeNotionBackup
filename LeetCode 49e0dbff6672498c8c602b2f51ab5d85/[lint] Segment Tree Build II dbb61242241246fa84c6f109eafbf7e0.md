# [lint] Segment Tree Build II

#: 439
Difficult: Medium
Tags: SegmentTree_idx
link: https://www.lintcode.com/problem/439
Priority: Low
Created time: August 28, 2022 5:36 PM
Last edited time: October 27, 2023 4:04 PM
practicedTimes: 1
source: jiuzhang, 算高SegmentTree

Description

The structure of Segment Tree is a binary tree which each node has two attributes `start` and `end` denote an segment / interval.

`start` and `end` are both integers, they should be assigned in following rules:

- The root's `start` and `end` is given by `build` method.
- The left child of node A has `start=A.left, end=(A.left + A.right) / 2`.
- The right child of node A has `start=(A.left + A.right) / 2 + 1, end=A.right`.
- if `start` equals to `end`, there will be no children for this node.

Implement a `build` method with a given array, so that we can create a corresponding segment tree with every node value represent the corresponding interval max value in the array, return the root of this segment tree.

# 

Segment Tree (a.k.a Interval Tree) is an advanced data structure which can support queries like:

- which of these intervals contain a given point
- which of these points are in a given interval

See wiki:[Segment Tree](https://en.wikipedia.org/wiki/Segment_tree)[Interval Tree](https://en.wikipedia.org/wiki/Interval_tree)

Example

`Input: [3,2,1,4]
Explanation:
The segment tree will be
          [0,3](max=4)
          /          \
       [0,1]         [2,3]    
      (max=3)       (max=4)
      /   \          /    \    
   [0,0]  [1,1]    [2,2]  [3,3]
  (max=3)(max=2)  (max=1)(max=4)`

```cpp
/**
 * Definition of SegmentTreeNode:
 * class SegmentTreeNode {
 * public:
 *     int start, end, max;
 *     SegmentTreeNode *left, *right;
 *     SegmentTreeNode(int start, int end, int max) {
 *         this->start = start;
 *         this->end = end;
 *         this->max = max;
 *         this->left = this->right = NULL;
 *     }
 * }
 */

class Solution {
public:
    /**
     * @param a: a list of integer
     * @return: The root of Segment Tree
     */
    SegmentTreeNode* build(vector<int> &nums) {
        int n=nums.size(); if(n==0) {return NULL;}
        return build(nums, 0, n-1);
    }

    SegmentTreeNode* build(vector<int>& nums, int start, int end){
        //leaf
        if(start==end) return new SegmentTreeNode(start, end, nums[start]);

        int mid=start+(end-start)/2;
        SegmentTreeNode* l = build(nums, start, mid);
        SegmentTreeNode* r = build(nums, mid+1, end);
        SegmentTreeNode* res = new SegmentTreeNode(start, end, max(l->max,r->max));
        res->left=l, res->right=r;
        return res;
    }
};
```