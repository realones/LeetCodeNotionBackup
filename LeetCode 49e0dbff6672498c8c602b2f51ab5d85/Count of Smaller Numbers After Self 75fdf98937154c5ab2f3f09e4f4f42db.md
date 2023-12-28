# Count of Smaller Numbers After Self

#: 315
Difficult: Hard
Tags: Divide and Conquer, MergeSort, SegmentTree_val, Tree
link: https://leetcode.com/problems/count-of-smaller-numbers-after-self/
Priority: High
Status: More Solution, No Idea At First
Created time: August 31, 2022 12:01 AM
Last edited time: December 18, 2023 11:10 PM
practicedTimes: 1
source: HuaHua, jiuzhang, 算高SegmentTree
related: Reverse Pairs (Reverse%20Pairs%20253d0403dba84fc2857f5c4546ebdd39.md)

Given an integer array `nums`, return *an integer array* `counts` *where* `counts[i]` *is the number of smaller elements to the right of* `nums[i]`.

**Example 1:**

```
Input: nums = [5,2,6,1]
Output: [2,1,1,0]
Explanation:
To the right of 5 there are2 smaller elements (2 and 1).
To the right of 2 there is only1 smaller element (1).
To the right of 6 there is1 smaller element (1).
To the right of 1 there is0 smaller element.

```

**Example 2:**

```
Input: nums = [-1]
Output: [0]

```

**Example 3:**

```
Input: nums = [-1,-1]
Output: [0,0]

```

**Constraints:**

- `1 <= nums.length <= 105`
- `104 <= nums[i] <= 104`

Solution mergesort:

```cpp
class Solution {
public:
    vector<int> res;
    vector<int> countSmaller(vector<int>& nums) {
        int n=nums.size();
        res.resize(n,0);
        vector<int> idx(n);
        for(int i=0; i<n; i++){idx[i]=i;}

        vector<int> tmp(n);
        mergeSort(nums, idx, tmp, 0, n-1);

        return res;
    }

    //sort idx
    void mergeSort(vector<int>& nums, vector<int>& idx, vector<int>& tmp, int start, int end){
        if(start>=end){return;}
        int m=start+(end-start)/2;
        mergeSort(nums,idx,tmp,start,m);
        mergeSort(nums,idx,tmp,m+1,end);
        merge(nums,idx,tmp,start,end);
    }

    void merge(vector<int>& nums, vector<int>&idx, vector<int>& tmp, int start, int end){
        if(start>=end){return;}
        int m=start+(end-start)/2;
        int l=m, r=end;
        int i=end;
        while(l>=start && r>=m+1){
            if(nums[idx[r]]>=nums[idx[l]]){tmp[i--]=idx[r--];}
            else{
                res[idx[l]]+=r-m;
                tmp[i--]=idx[l--];
            }
        }
        while(r>=m+1){
            tmp[i--]=idx[r--];
        }
        while(l>=start){
            tmp[i--]=idx[l--];
        }

        for(int k=start;k<=end;k++){
            idx[k]=tmp[k];
        }
    }
};
```

**Solution 1**:

SegTree_val

From right to left:

1. 1. insert an element.
2. 2. query a range

```cpp
class Solution {
public:
    
    class Node{
        public:
            int start, end, cnt;
            Node* left, *right;
            Node(int start, int end, int cnt): start(start), end(end), cnt(cnt), left(NULL), right(NULL){}
    };
    
    Node* build(int start, int end){
        //if leaf
        if(start==end) return new Node(start, end, 0);
        //not leaf
        int mid=start+(end-start)/2;
        Node* res=new Node(start, end, 0);
        res->left=build(start, mid);
        res->right=build(mid+1, end);
        return res;
    }
    
    int query(Node* root, int start, int end){
        //if invalid: e.g. last element is smallest
        if(start>end){return 0;}
        //if match
        if(start==root->start && end==root->end){
            return root->cnt;
        }
        //contains
        int mid=root->start+(root->end - root->start)/2;
        if(end<=mid){return query(root->left, start ,end);}
        else if(start>mid){return query(root->right, start, end);}
        else{
            int l=query(root->left, start ,mid);
            int r=query(root->right, mid+1 ,end);
            return l+r;
        }
    }
    
    void insert(Node* root, int val){
        //find
        if(val==root->start && val==root->end){root->cnt++; return;}
        //contain
        int mid=root->start+(root->end - root->start)/2;
        if(val<=mid){
            insert(root->left, val);
        }
        else{
            insert(root->right, val);
        }
        root->cnt++;
    }
    
    vector<int> countSmaller(vector<int>& nums) {
        int n=nums.size(); if(n==0) {return {};}
        vector<int> res;
        //build segment tree on val from min to max, init cnt=0
        int minVal = *min_element(nums.begin(), nums.end());
        int maxVal = *max_element(nums.begin(), nums.end());
        Node* root = build(minVal,maxVal);
        //from last to first in nums, update in segment tree, add query to res
        reverse(nums.begin(),nums.end());
        for(auto num: nums){
            res.push_back(query(root,minVal,num-1));
            insert(root,num);
        }
        //reverse res and return
        reverse(res.begin(),res.end());
        return res;
    }
};
```