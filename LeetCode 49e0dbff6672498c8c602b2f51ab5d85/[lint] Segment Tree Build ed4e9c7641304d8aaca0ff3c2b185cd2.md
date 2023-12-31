# [lint] Segment Tree Build

#: 201
Difficult: Medium
Tags: SegmentTree_idx
link: https://www.lintcode.com/problem/201/
Priority: Pass
Created time: August 28, 2022 5:30 PM
Last edited time: October 26, 2023 4:32 PM
practicedTimes: 1
source: jiuzhang, 算高SegmentTree

Description

The structure of Segment Tree is a binary tree which each node has two attributes `start` and `end` denote an segment / interval.

*start* and *end* are both integers, they should be assigned in following rules:

- The root's *start* and *end* is given by `build` method.
- The left child of node A has `start=A.start, end=(A.start + A.end) / 2`.
- The right child of node A has `start=(A.start + A.end) / 2 + 1, end=A.end`.
- if *start* equals to *end*, there will be no children for this node.

Implement a `build` method with two parameters *start* and *end*, so that we can create a corresponding segment tree with every node has the correct *start* and *end* value, return the root of this segment tree.

# 

Segment Tree (a.k.a Interval Tree) is an advanced data structure which can support queries like:

- which of these intervals contain a given point
- which of these points are in a given interval

See wiki:[Segment Tree](https://en.wikipedia.org/wiki/Segment_tree)[Interval Tree](https://en.wikipedia.org/wiki/Interval_tree)

Example

**Example 1:**

```
Input：[1,4]
Output："[1,4][1,2][3,4][1,1][2,2][3,3][4,4]"
Explanation：
	               [1,  4]
	             /        \
	      [1,  2]           [3, 4]
	      /     \           /     \
	   [1, 1]  [2, 2]     [3, 3]  [4, 4]

```

**Example 2:**

```
Input：[1,6]
Output："[1,6][1,3][4,6][1,2][3,3][4,5][6,6][1,1][2,2][4,4][5,5]"
Explanation：
	       [1,  6]
             /        \
      [1,  3]           [4,  6]
      /     \           /     \
   [1, 2]  [3,3]     [4, 5]   [6,6]
   /    \           /     \
[1,1]   [2,2]     [4,4]   [5,5]

```

Build tree:

字诀:

- 自上而下递归分裂
- 自下而上回溯更新

时间复杂度是多少?

以数组下标来建立线段树

对比一下堆建立和线段树建立后树的区别和点个数区别

```cpp
/**
 * Definition of SegmentTreeNode:
 * class SegmentTreeNode {
 * public:
 *     int start, end;
 *     SegmentTreeNode *left, *right;
 *     SegmentTreeNode(int start, int end) {
 *         this->start = start, this->end = end;
 *         this->left = this->right = NULL;
 *     }
 * }
 */

class Solution {
public:
    /*
     * @param start: start value.
     * @param end: end value.
     * @return: The root of Segment Tree.
     */
    SegmentTreeNode * build(int start, int end) {
        if(start>end) return NULL;

        //if leaf
        if(start==end) return new SegmentTreeNode(start, end);

        int mid=start+(end-start)/2;
        SegmentTreeNode* l=build(start, mid);
        SegmentTreeNode* r=build(mid+1, end);
        SegmentTreeNode* res = new SegmentTreeNode(start, end);
        res->left=l, res->right=r;
        return res;
    }
};
```