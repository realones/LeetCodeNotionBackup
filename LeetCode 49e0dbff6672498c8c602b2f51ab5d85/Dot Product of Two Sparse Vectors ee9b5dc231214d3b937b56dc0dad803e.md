# Dot Product of Two Sparse Vectors

#: 1570
Difficult: Medium
Tags: 2pt, Array, Design, Hash
link: https://leetcode.com/problems/dot-product-of-two-sparse-vectors/
Priority: Medium
Created time: November 29, 2023 11:58 PM
Last edited time: November 29, 2023 11:59 PM
source: facebookFreq

Given two sparse vectors, compute their dot product.

Implement class `SparseVector`:

- `SparseVector(nums)` Initializes the object with the vector `nums`
- `dotProduct(vec)` Compute the dot product between the instance of *SparseVector* and `vec`

A **sparse vector** is a vector that has mostly zero values, you should store the sparse vector **efficiently** and compute the dot product between two *SparseVector*.

**Follow up:** What if only one of the vectors is sparse?

**Example 1:**

```
Input: nums1 = [1,0,0,2,3], nums2 = [0,3,0,4,0]
Output: 8
Explanation: v1 = SparseVector(nums1) , v2 = SparseVector(nums2)
v1.dotProduct(v2) = 1*0 + 0*3 + 0*0 + 2*4 + 3*0 = 8

```

**Example 2:**

```
Input: nums1 = [0,1,0,0,0], nums2 = [0,0,0,0,2]
Output: 0
Explanation: v1 = SparseVector(nums1) , v2 = SparseVector(nums2)
v1.dotProduct(v2) = 0*0 + 1*0 + 0*0 + 0*0 + 0*2 = 0

```

**Example 3:**

```
Input: nums1 = [0,1,0,0,2,0,0], nums2 = [1,0,0,0,3,0,4]
Output: 6

```

**Constraints:**

- `n == nums1.length == nums2.length`
- `1 <= n <= 10^5`
- `0 <= nums1[i], nums2[i] <= 100`

```cpp
using ii=pair<int,int>;

class SparseVector {
public:
    vector<ii> v;//idx, val!=0

    SparseVector(vector<int> &nums) {
        int n=nums.size();// if(n==0) {return 0;}
        for(int i=0; i<n; i++){
            if(nums[i]) v.emplace_back(i,nums[i]);
        }
    }
    
    // Return the dotProduct of two sparse vectors
    int dotProduct(SparseVector& vec) {
        if(v.empty() || vec.v.empty()) return 0;
        int res=0;
        int i=0, j=0;
        while(i<v.size() && j<vec.v.size()){
            int idx1=v[i].first;
            int idx2=vec.v[j].first;
            if(idx1<idx2) i++;
            else if(idx1>idx2) j++;
            else res+=v[i++].second*vec.v[j++].second;
        }
        return res;
    }
};

// Your SparseVector object will be instantiated and called as such:
// SparseVector v1(nums1);
// SparseVector v2(nums2);
// int ans = v1.dotProduct(v2);

/*
len: 1~1e5
val: 0~100
================================
nums(mostly zero vals)-> tmp 
calcu nums1 .* nums2
================================
*/
```