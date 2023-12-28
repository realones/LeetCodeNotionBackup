# Range Sum Query - Mutable

#: 307
Difficult: Medium
Tags: Array, BinaryIndexedTree, Design, SegmentTree_idx
link: https://leetcode.com/problems/range-sum-query-mutable/
Priority: Low
Status: More Solution
Created time: September 11, 2023 5:30 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given an integer array `nums`, handle multiple queries of the following types:

1. **Update** the value of an element in `nums`.
2. Calculate the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** where `left <= right`.

Implement the `NumArray` class:

- `NumArray(int[] nums)` Initializes the object with the integer array `nums`.
- `void update(int index, int val)` **Updates** the value of `nums[index]` to be `val`.
- `int sumRange(int left, int right)` Returns the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** (i.e. `nums[left] + nums[left + 1] + ... + nums[right]`).

**Example 1:**

```
Input
["NumArray", "sumRange", "update", "sumRange"]
[[[1, 3, 5]], [0, 2], [1, 2], [0, 2]]
Output
[null, 9, null, 8]

Explanation
NumArray numArray = new NumArray([1, 3, 5]);
numArray.sumRange(0, 2); // return 1 + 3 + 5 = 9
numArray.update(1, 2);   // nums = [1, 2, 5]
numArray.sumRange(0, 2); // return 1 + 2 + 5 = 8

```

**Constraints:**

- `1 <= nums.length <= 3 * 104`
- `100 <= nums[i] <= 100`
- `0 <= index < nums.length`
- `100 <= val <= 100`
- `0 <= left <= right < nums.length`
- At most `3 * 104` calls will be made to `update` and `sumRange`.

```cpp
class Node{
public:
    int start, end, sum;
    Node *left, *right;

    Node(int start, int end, int sum){
        this->start=start;
        this->end=end;
        this->sum=sum;
        this->left=NULL, this->right=NULL;
    }
};

class NumArray {
public:
    Node* root;

    NumArray(vector<int>& nums) {
        root = build(nums,0,nums.size()-1);
    }
    Node* build(vector<int>& nums, int start, int end){
        if(start==end) return new Node(start, end, nums[start]);
        int mid=start+(end-start)/2;
        Node* root=new Node(start,end,0);
        root->left=build(nums,start,mid);
        root->right=build(nums,mid+1,end);
        root->sum=root->left->sum+root->right->sum;
        return root;
    }
    
    void update(int index, int val) {
        update(root, index, val);
    }
    void update(Node* root, int index, int val){
        if(root->start == index && root->end == index) {
            root->sum=val;
            return;
        }

        int mid=root->start+(root->end - root->start)/2;
        //left
        if(index<=mid){
            update(root->left, index, val);
        }
        //right
        else{
            update(root->right, index, val);
        }
        root->sum = root->left->sum + root->right->sum;
    }
    
    int sumRange(int start, int end) {
        return query(root, start, end);
    }

    int query(Node* root, int start, int end) {
        if(start==root->start && end==root->end) return root->sum;

        int mid=root->start+(root->end - root->start)/2;
        //left
        if(end<=mid){
            return query(root->left, start, end);
        }
        //right
        else if(start>=mid+1){
            return query(root->right, start ,end);
        }
        //both
        else{
            return query(root->left, start, mid) + query(root->right, mid+1, end);
        }
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * obj->update(index,val);
 * int param_2 = obj->sumRange(left,right);
 */
```