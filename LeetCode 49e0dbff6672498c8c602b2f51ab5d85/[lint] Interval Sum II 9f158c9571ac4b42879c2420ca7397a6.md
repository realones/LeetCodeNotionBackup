# [lint] Interval Sum II

#: 207
Difficult: Hard
Tags: SegmentTree_idx
link: https://www.lintcode.com/problem/207/
Priority: Low
Created time: August 30, 2022 11:46 AM
Last edited time: October 27, 2023 10:29 PM
practicedTimes: 1
source: jiuzhang, 算高SegmentTree

Description

Given an integer array in the construct method, implement two methods `query(start, end)` and `modify(index, value)`:

- For query(*start*, *end*), return the **sum** from index *start* to index *end* in the given array.
- For modify(*index*, *value*), modify the number in the given index to *value*

# 

We suggest you finish problem [Segment Tree Build](https://www.lintcode.com/problem/segment-tree-build/), [Segment Tree Query](https://www.lintcode.com/problem/segment-tree-query/) and [Segment Tree Modify](https://www.lintcode.com/problem/segment-tree-modify/) first.

Example

**Example 1**

```
Input:
[1,2,7,8,5]
[query(0,2),modify(0,4),query(0,1),modify(2,1),query(2,4)]
Output: [10,6,14]
Explanation:
Given array A = [1,2,7,8,5].
After query(0, 2), 1 + 2 + 7 = 10,
After modify(0, 4), change A[0] from 1 to 4, A = [4,2,7,8,5].
After query(0, 1), 4 + 2 = 6.
After modify(2, 1), change A[2] from 7 to 1，A = [4,2,1,8,5].
After query(2, 4), 1 + 8 + 5 = 14.

```

**Example 2**

```
Input:
[1,2,3,4,5]
[query(0,0),query(1,2),quert(3,4)]
Output: [1,5,9]
Explantion:
1 = 1
2 + 3 = 5
4 + 5 = 9

```

Challenge

O(logN) time for `query` and `modify`.

```cpp
//!!fake answer => find the time limited exceeded one

class SegmentTree{
public:
    int start, end;
    long long sum;
    SegmentTree* left, *right;
    SegmentTree(int start, int end, long long sum = 0): start(start),end(end),sum(sum){};
    
    static SegmentTree* build(int start, int end, vector<int> &A) {
        if (start > end) return nullptr;
        SegmentTree* head = new SegmentTree(start, end, A[start]);
        if (start == end) return head;
        int mid = (start + end) /2;
        head->left = build(start, mid, A);
        head->right = build(mid+1, end, A);
        
        head->sum = 0;
        if (head->left)
            head->sum += head->left->sum;
        if (head->right)
            head->sum += head->right->sum;
        return head;
    }
    
    static long long query(SegmentTree* head, int start, int end) {
        if (start > head->end || end < head->start) return 0;
        if (start <= head->start && end >= head->end) {
            return head->sum;
        }
        int l = query(head->left, start, end);
        int r = query(head->right, start, end);
        return l + r;
    }
    
    static void modify(SegmentTree* head, int index, int value) {
        if (head->start == head->end) {
            head->sum = value;
            return;
        }
        int mid = (head->start + head->end) /2;
        if (index > mid) {
            modify(head->right, index, value);
        }
        else if(index <= mid) {
            modify(head->left, index, value);
        }
        
        head->sum = 0;
        if (head->left)
            head->sum += head->left->sum;
        if (head->right)
            head->sum += head->right->sum;
    }
};

class Solution {
public:
    /* you may need to use some attributes here */
    SegmentTree* head;

    /**
     * @param A: An integer vector
     */
    Solution(vector<int> A) {
        // write your code here
        head = SegmentTree:: build(0, A.size()-1, A);
    }
    
    /**
     * @param start, end: Indices
     * @return: The sum from start to end
     */
    long long query(int start, int end) {
        // write your code here
        long long res = SegmentTree::query(head, start, end);
        return res;
    }
    
    /**
     * @param index, value: modify A[index] to value.
     */
    void modify(int index, int value) {
        // write your code here
        SegmentTree::modify(head, index, value);
    }
};
```