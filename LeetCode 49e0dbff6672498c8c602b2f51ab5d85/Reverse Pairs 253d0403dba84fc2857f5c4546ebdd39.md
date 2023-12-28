# Reverse Pairs

#: 493
Difficult: Hard
Tags: Array, BS_todo, BinaryIndexedTree, Divide and Conquer, MergeSort, OrderSet, SegmentTree_todo
link: https://leetcode.com/problems/reverse-pairs/description/
Priority: High
Status: out of scope, toSummarize, todo
Created time: December 18, 2023 10:16 PM
Last edited time: December 19, 2023 7:15 PM
source: googleFreq
related: Find Building Where Alice and Bob Can Meet (Find%20Building%20Where%20Alice%20and%20Bob%20Can%20Meet%20541c5473515546d8adc8aa52d8893d9f.md), Count of Smaller Numbers After Self (Count%20of%20Smaller%20Numbers%20After%20Self%2075fdf98937154c5ab2f3f09e4f4f42db.md)

Given an integer array `nums`, return *the number of **reverse pairs** in the array*.

A **reverse pair** is a pair `(i, j)` where:

- `0 <= i < j < nums.length` and
- `nums[i] > 2 * nums[j]`.

**Example 1:**

```
Input: nums = [1,3,2,3,1]
Output: 2
Explanation: The reverse pairs are:
(1, 4) --> nums[1] = 3, nums[4] = 1, 3 > 2 * 1
(3, 4) --> nums[3] = 3, nums[4] = 1, 3 > 2 * 1

```

**Example 2:**

```
Input: nums = [2,4,3,5,1]
Output: 3
Explanation: The reverse pairs are:
(1, 4) --> nums[1] = 4, nums[4] = 1, 4 > 2 * 1
(2, 4) --> nums[2] = 3, nums[4] = 1, 3 > 2 * 1
(3, 4) --> nums[3] = 5, nums[4] = 1, 5 > 2 * 1

```

**Constraints:**

- `1 <= nums.length <= 5 * 104`
- `231 <= nums[i] <= 231 - 1`

todo: [https://leetcode.com/problems/reverse-pairs/solutions/97268/general-principles-behind-problems-similar-to-reverse-pairs/](https://leetcode.com/problems/reverse-pairs/solutions/97268/general-principles-behind-problems-similar-to-reverse-pairs/)

OrderSet - TLE:

```cpp
using ll=long long;
class Solution {
public:
    int reversePairs(vector<int>& nums) {
        int res=0;
        multiset<ll> st;
        for(auto x: nums){
            //it's a pity orderSet iteration distance call is not T:O(1)
            res+= distance(st.upper_bound(2LL*x), st.end());
            st.insert(x);
        }
        return res;
    }
};

/*
valid pair:
    i<j
    nums[i] > 2*nums[j]
return:
    num of valid pairs
================================
nums:
    len: 5e4
    val: INT_MIN ~ INT_MAX
================================
no sort
for nums:
    find [0~i-1], cnt of elem > 2*cur
    find by idx range [0~i-1], val range (2*nums[i], infinite)
    build segtree, OrderSet, mono stk
OrderSet:
    for each num
        check how many elem in orderSet > val*2
        put cur val into orderSet
*/
```

SegTree_val - MLE

```cpp
using ll=long long;

class Node {
public:
    ll start, end;
    ll cnt;
    Node *left, *right;
    Node(ll start, ll end, ll cnt=0, Node* left=NULL, Node* right=NULL):start(start), end(end), cnt(cnt), left(left), right(right) {}
};

class Solution {
public:

    Node* build(ll start, ll end) {
        //leaf
        if(start==end) return new Node(start, end);
    
        ll mid=start+(end-start)/2;
        Node* l=build(start, mid);
        Node* r=build(mid+1, end);
        return new Node(start, end, 0, l, r);
    }

    ll query(Node * root, ll start, ll end) {
        if(start>end) return 0;//validate param start and end
        if(start==root->start && end==root->end) return root->cnt;

        ll mid=root->start+(root->end-root->start)/2;
        if(end<=mid){
            return query(root->left,start,end);
        }
        else if(start>mid){
            return query(root->right,start,end);
        }
        else {
            ll l=query(root->left,start,mid);
            ll r=query(root->right,mid+1,end);
            return l+r;
        }
    }

    void insert(Node * root, ll value) {
        //find leaf
        if(value==root->start && value==root->end){
            root->cnt++; 
            return;
        }
        //contains
        ll mid=root->start+(root->end-root->start)/2;
        if(value<=mid){
            insert(root->left, value);
        }
        if(value>=mid+1){
            insert(root->right, value);
        }
        root->cnt=root->left->cnt + root->right->cnt;
    }

    int reversePairs(vector<int>& nums) {
        ll res=0;
        int minEle=*min_element(nums.begin(), nums.end());
        int maxEle=*max_element(nums.begin(), nums.end());
        Node* root = build(minEle, maxEle);
        for(auto x: nums){
            res+= query(root, 2*x+1, maxEle);
            insert(root,x);
        }
        return res;
    }
};

/*
valid pair:
    i<j
    nums[i] > 2*nums[j]
return:
    num of valid pairs
================================
nums:
    len: 5e4
    val: ll_MIN ~ ll_MAX
================================
no sort
for nums:
    find [0~i-1], cnt of elem > 2*cur
    find by idx range [0~i-1], val range (2*nums[i], infinite)
    build segtree, OrderSet, mono stk
OrderSet:
    for each num
        check how many elem in orderSet > val*2
        put cur val llo orderSet
*/
```