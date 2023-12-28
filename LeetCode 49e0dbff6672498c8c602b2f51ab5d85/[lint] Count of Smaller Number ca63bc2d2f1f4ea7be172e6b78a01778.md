# [lint] Count of Smaller Number

#: 248
Difficult: Medium
Tags: BS_lower_upper_bound, SegmentTree_val, Sort
link: http://www.lintcode.com/en/problem/count-of-smaller-number
Priority: Medium
Status: toRevisit
Created time: August 30, 2022 4:18 PM
Last edited time: October 28, 2023 12:44 PM
practicedTimes: 1
source: jiuzhang, 算高SegmentTree

Description

Give you an integer array (index from 0 to n-1, where n is the size of this array, value from 0 to 10000) and an query list. For each query, give you an integer, return the number of elements in the array that are smaller than the given integer.

---

We suggest you finish problem [Segment Tree Build](https://www.lintcode.com/problem/segment-tree-build/) and [Segment Tree Query II](https://www.lintcode.com/problem/segment-tree-query-ii/) first.

Example

**Example 1:**

```
Input: array =[1,2,7,8,5] queries =[1,8,5]
Output:[0,4,2]

```

**Example 2:**

```
Input: array =[3,4,5,8] queries =[2,4]
Output:[0,1]

```

Challenge

Could you use three ways to do it.

1. Just loop
2. Sort and binary search
3. Build Segment Tree and Search.

1. for 循环遍历 o(n)
2. 排序二分 o(log(n))
3. 线段树 o(log(m)) - 比较难想，属于以数值来建立线段树

```cpp
// Binary Search Version
class Solution {
public:
    vector<int> countOfSmallerNumber(vector<int> &A, vector<int> &queries) {
        // write your code here
        vector<int> res;
        sort(A.begin(), A.end());
        for(auto& q: queries){
            auto low=std::lower_bound(A.begin(), A.end(), q);
            int tmp=low-A.begin();
            res.push_back(tmp);
        }
        return res;
    }
};
```

```cpp
// Segment Tree Version
class Solution {
public:
    /**
     * @param A: An integer array
     * @return: The number of element in the array that
     *          are smaller that the given integer
     */
    class SegmentTreeNode {
        public:
            int start, end, count;
            SegmentTreeNode *left, *right;
            SegmentTreeNode(int start, int end, int count) {
                this->start = start;
                this->end = end;
                this->count = count;
                this->left = this->right = NULL;
        }
    };

    SegmentTreeNode *build(int start, int end) {
        SegmentTreeNode *root = new SegmentTreeNode(start, end, 0);
        if (start == end) {return root;}
        int mid = start + (end - start) / 2;
        root->left = build(start, mid);
        root->right = build(mid + 1, end);
        return root;
    }

    int query(SegmentTreeNode *root, int start, int end) {
        if(start==root->start && end==root->end) return root->count;
        int m=root->start+(root->end-root->start)/2;
        if(end<=m){
            return query(root->left,start,end);
        }
        if(start>=m+1){
            return query(root->right,start,end);
        }
        if(start<=m && end>=m+1){
            int l=query(root->left,start,m);
            int r=query(root->right,m+1,end);
            return l+r;
        }
    }
    
    void insert(SegmentTreeNode *root, int val) {
        if(root->start==val && root->end==val) {root->count++; return;}
        int m=root->start+(root->end-root->start)/2;
        if(val<=m){
            insert(root->left,val);
        }
        if(val>=m+1){
            insert(root->right,val);
        }
        root->count++;
    }
    
    //main
    vector<int> countOfSmallerNumber(vector<int> &A, vector<int> &queries) {
        vector<int> result;
        SegmentTreeNode *root = build(0, 10000);
        for (const auto& num : A) {
            insert(root, num);
        }
        for (const auto& q : queries) {
            result.push_back(query(root, 0, q - 1));
        }
        return result;
    }
};
```