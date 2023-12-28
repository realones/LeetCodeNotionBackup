# [lint] Segment Tree Query

#: 202
Difficult: Medium
Tags: SegmentTree_idx
link: https://www.lintcode.com/problem/segment-tree-query
Priority: Pass
Created time: August 28, 2022 11:14 AM
Last edited time: October 27, 2023 4:54 PM
practicedTimes: 1
source: jiuzhang, 算高SegmentTree

For an integer array (index from 0 to n-1, where n is the size of this array), in the corresponding SegmentTree, each node stores an extra attribute `max` to denote the maximum number in the interval of the array (index from start to end).

Design a `query` method with three parameters `root`, `start` and `end`,  find the maximum number in the interval [start, end] by the given root of segment tree.

Example:
**Example 1:**

```
Input："[0,3,max=4][0,1,max=4][2,3,max=3][0,0,max=1][1,1,max=4][2,2,max=2][3,3,max=3]",1,2
Output：4
Explanation：
For array [1, 4, 2, 3], the corresponding Segment Tree is :

	                  [0, 3, max=4]
	                 /             \\
	          [0,1,max=4]        [2,3,max=3]
	          /         \\        /         \\
	   [0,0,max=1] [1,1,max=4] [2,2,max=2], [3,3,max=3]
The maximum value of [1,2] interval is 4

```

**Example 2:**

```
Input："[0,3,max=4][0,1,max=4][2,3,max=3][0,0,max=1][1,1,max=4][2,2,max=2][3,3,max=3]",2,3
Output：3
Explanation：
For array [1, 4, 2, 3], the corresponding Segment Tree is :

	                  [0, 3, max=4]
	                 /             \\
	          [0,1,max=4]        [2,3,max=3]
	          /         \\        /         \\
	   [0,0,max=1] [1,1,max=4] [2,2,max=2], [3,3,max=3]
The maximum value of [2,3] interval is 3

```

节点区间和要查找区间的关系

四种情况:

1. 节点区间包含查找区间—>查找区间递归向下
2. 节点区间不相交于查找区间 ->查找区间停止搜索
3. 节点区间相交不包含于查找区间->查找区间分裂成两段区间，一段于被节点区间包含，另一段不相交
4. 节点区间相等于查找区间—> 返回值查找的结果

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
    /*
     * @param root: The root of segment tree.
     * @param start: start value.
     * @param end: end value.
     * @return: The maximum number in the interval [start, end]
     */
    int query(SegmentTreeNode * root, int start, int end) {
        //should have some sanity check here

        //no intersection
        //if(start>root->end || end<root->start) return INT_MIN;
        //if same
        if(start==root->start && end==root->end) return root->max;
        //if contains
        int mid=root->start+(root->end-root->start)/2;
		    if(end<=mid){
		        return query(root->left,start,end);
		    }
		    else if(start>mid){
		        return query(root->right,start,end);
		    }
		    else {
		        int l=query(root->left,start,mid);
		        int r=query(root->right,mid+1,end);
		        return max(l,r);
		    }
    }
};
```