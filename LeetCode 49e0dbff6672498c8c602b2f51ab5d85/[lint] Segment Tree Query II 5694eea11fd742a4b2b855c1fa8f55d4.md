# [lint] Segment Tree Query II

#: 247
Difficult: Medium
Tags: SegmentTree_idx
link: https://www.lintcode.com/problem/segment-tree-query-ii/
Priority: Low
Created time: August 30, 2022 12:27 PM
Last edited time: October 27, 2023 10:31 PM
practicedTimes: 1
source: jiuzhang, 算高SegmentTree

For an array, we can build a `SegmentTree` for it, each node stores an extra attribute `count` to denote the number of elements in the the array which value is between interval start and end. (The array may not fully filled by elements)

Design a `query` method with three parameters `root`, `start` and `end`,  find the number of elements in the in array's interval [*start*, *end*] by the given root of value SegmentTree.

Example:
**Example 1:**

```
Input："[0,3,count=3][0,1,count=1][2,3,count=2][0,0,count=1][1,1,count=0][2,2,count=1][3,3,count=1]",[[1, 1], [1, 2], [2, 3], [0, 2]]
Output：[0,1,2,2]
Explanation：
The corresponding value Segment Tree is:

	                     [0, 3, count=3]
	                     /             \\
	          [0,1,count=1]             [2,3,count=2]
	          /         \\               /            \\
	   [0,0,count=1] [1,1,count=0] [2,2,count=1], [3,3,count=1]

Input : query(1,1), Output: 0

Input : query(1,2), Output: 1

Input : query(2,3), Output: 2

Input : query(0,2), Output: 2

```

**Example 2:**

```
Input："[0,3,count=3][0,1,count=1][2,3,count=2][0,0,count=1][1,1,count=0][2,2,count=0][3,3,count=1]",[[1, 1], [1, 2], [2, 3], [0, 2]]
Output：[0,0,1,1]
Explanation：
The corresponding value Segment Tree is:

	                     [0, 3, count=2]
	                     /             \\
	          [0,1,count=1]             [2,3,count=1]
	          /         \\               /            \\
	   [0,0,count=1] [1,1,count=0] [2,2,count=0], [3,3,count=1]

Input : query(1,1), Output: 0

Input : query(1,2), Output: 0

Input : query(2,3), Output: 1

Input : query(0,2), Output: 1

```

```cpp
/**
 * Definition of SegmentTreeNode:
 * class SegmentTreeNode {
 * public:
 *     int start, end, count;
 *     SegmentTreeNode *left, *right;
 *     SegmentTreeNode(int start, int end, int count) {
 *         this->start = start;
 *         this->end = end;
 *         this->count = count;
 *         this->left = this->right = NULL;
 *     }
 * }
 */

class Solution {
public:
    /*
     * @param root: The root of segment tree.
     * @param start: start value.
     * @param end: end value.
     * @return: The count number in the interval [start, end]
     */
    int query(SegmentTreeNode * root, int start, int end) {
        // write your code here
        if(!root ) return 0;
        start=max(root->start,start);
        end=min(root->end,end);
        if(start>end) return 0;
        return query2(root,start,end);
    }

    int query2(SegmentTreeNode * root, int start, int end) {
        if(root->start==start && root->end==end){
            return root->count;
        }
        int m=root->start+(root->end-root->start)/2;
        if(end<=m){
            return query2(root->left,start,end);
        }
        if(start>=m+1){
            return query2(root->right,start,end);;
        }
        if(start<=m && end>=m+1){
            int l=query2(root->left,start,m);
            int r=query2(root->right,m+1,end);
            return l+r;
        }
    }
};
```