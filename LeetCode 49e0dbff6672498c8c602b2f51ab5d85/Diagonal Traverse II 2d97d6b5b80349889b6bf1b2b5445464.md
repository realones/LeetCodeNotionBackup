# Diagonal Traverse II

#: 1424
Difficult: Medium
Tags: Array, PQ, Sort, matrix-diagTraverse
link: https://leetcode.com/problems/diagonal-traverse-ii/
Priority: Medium
Status: HardToImplement, More Solution
Created time: June 27, 2023 9:22 PM
Last edited time: November 21, 2023 5:03 PM
source: HuaHua, leetcode_daily

Given a 2D integer array `nums`, return *all elements of* `nums` *in diagonal order as shown in the below images*.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/04/08/sample_1_1784.png](https://assets.leetcode.com/uploads/2020/04/08/sample_1_1784.png)

```
Input: nums = [[1,2,3],[4,5,6],[7,8,9]]
Output: [1,4,2,7,5,3,8,6,9]

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/04/08/sample_2_1784.png](https://assets.leetcode.com/uploads/2020/04/08/sample_2_1784.png)

```
Input: nums = [[1,2,3,4,5],[6,7],[8],[9,10,11],[12,13,14,15,16]]
Output: [1,6,2,8,7,3,9,4,12,10,5,13,11,14,15,16]

```

**Constraints:**

- `1 <= nums.length <= 105`
- `1 <= nums[i].length <= 105`
- `1 <= sum(nums[i].length) <= 105`
- `1 <= nums[i][j] <= 105`

comment: first use matrix-diag traverse but timeout, then use solution 1, but take more time than i expected, it is hard to implement

there are sorting and pq tag, so check more solutions

Solution 1:

```cpp
class Solution {
public:
    vector<int> findDiagonalOrder(vector<vector<int>>& matrix) {
        //get m,n,dist
        int m=matrix.size();//check case ==0
        int n=0;
        int dist=0;
        for(int i=0;i<m;i++){
            n=max(n,(int)matrix[i].size());
            dist=max(dist, i+1+n-2);
        }
        //process
        vector<vector<int>> ds(dist+1);
        for(int i=0;i<m;i++){
            for(int j=0;j<matrix[i].size();j++){
                int d=i+j;
                ds[d].push_back(matrix[i][j]);
            }
        }
        for(auto& v: ds) reverse(v.begin(), v.end());
        vector<int> res;
        for(auto& v: ds){
            res.insert(res.end(), v.begin(), v.end());
        }
        return res;
    }
};
```

Solution TLE:

```cpp
class Solution {
public:
    vector<int> findDiagonalOrder(vector<vector<int>>& matrix) {
        int m=matrix.size();//check case ==0
        int n=0;
        int dist=0;
        for(int i=0;i<m;i++){
            n=max(n,(int)matrix[i].size());
            dist=max(dist, i+1+n-2);
        }
        vector<int> res;
        for(int d=0; d<=dist; d++){
            for(int i=min(m-1, d),j; j=d-i, i>=max(d-n+1,0); i--){
                if(j<matrix[i].size()){
                    res.push_back(matrix[i][j]);
                }
            }
        }
        return res;
    }
};
```