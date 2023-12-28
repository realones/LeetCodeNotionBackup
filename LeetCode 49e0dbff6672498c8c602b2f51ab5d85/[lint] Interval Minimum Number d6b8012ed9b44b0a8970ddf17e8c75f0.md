# [lint] Interval Minimum Number

#: 205
Difficult: Medium
Tags: SegmentTree_idx
link: https://www.lintcode.com/problem/205/
Priority: Low
Status: More Solution
Created time: August 29, 2022 5:30 PM
Last edited time: October 28, 2023 12:36 PM
practicedTimes: 1
source: jiuzhang, 算高SegmentTree

Description

Given an integer array (index from 0 to n-1, where n is the size of this array), and an query list. Each query has two integers `[start, end]`. For each query, calculate the minimum number between index start and end in the given array,and return in the result list.

# 

We suggest you finish problem [Segment Tree Build](https://www.lintcode.com/problem/segment-tree-build/), [Segment Tree Query](https://www.lintcode.com/problem/segment-tree-query/) and [Segment Tree Modify](https://www.lintcode.com/problem/segment-tree-modify/) first.

Example

**Example 1:**

```
Input : array: [1,2,7,8,5], queries :[(1,2),(0,4),(2,4)] .Output: [2,1,5]

```

**Example 2:**

```
Input : array: [4,5,7,1], queries :[(1,2),(1,3)]. Output: [5,1]

```

Challenge

O(logN) time for each query

```cpp
/**
 * Definition of Interval:
 * class Interval {
 *     int start, end;
 *     Interval(int start, int end) {
 *         this->start = start;
 *         this->end = end;
 *     }
 * }
 */

class SegmentTree {
public:
	SegmentTree *left, *right;
	int start, end, minVal;
	SegmentTree(int start, int end, int minVal = 0) : left(NULL), right(NULL), start(start), end(end), minVal(minVal) {};

	static SegmentTree *build(vector<int> &A, int start, int end) {//assume input valid
		SegmentTree *root = new SegmentTree(start, end, A[start]);
		if (start == end) return root;
		int m = start + (end - start) / 2;
		root->left = build(A, start, m);
		root->right = build(A, m + 1, end);
		root->minVal = min(root->left->minVal, root->right->minVal);
		return root;
	}

	static int query(SegmentTree *root, int start, int end) {//assume input valid
		//same
		if (start == root->start && end == root->end) return root->minVal;
		//contains
		int m = root->start + (root->end - root->start) / 2;
		if (end <= m){
			return query(root->left, start, end);
		}
		if (start >= m + 1){
			return query(root->right, start, end);
		}
		if (start <= m && end >= m + 1){
			int l = query(root->left, start, m);
			int r = query(root->right, m + 1, end);
			return min(l, r);
		}
	}
};

class Solution {
public:
	/*
	* @param A: An integer array
	* @param queries: An query list
	* @return: The result list
	*/
	vector<int> intervalMinNumber(vector<int> &A, vector<Interval> &queries) {
		vector<int> res;
		int n = A.size(); if (n == 0) return res;

		SegmentTree *root = SegmentTree::build(A, 0, n - 1);

		for (auto& itv : queries){
			//sanity check
			int l = max(itv.start, 0);
			int r = min(itv.end, n - 1);
			if (l>r) { res.push_back(INT_MAX); continue; }

			res.push_back(SegmentTree::query(root, l, r));
		}
		return res;
	}

};
```