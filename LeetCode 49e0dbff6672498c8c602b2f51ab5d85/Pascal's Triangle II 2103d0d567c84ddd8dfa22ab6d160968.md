# Pascal's Triangle II

#: 119
Difficult: Easy
Tags: Array, DP
link: https://leetcode.com/problems/pascals-triangle-ii/
Priority: Medium
Created time: October 16, 2023 1:48 PM
Last edited time: October 16, 2023 1:48 PM
source: leetcode_daily

Given an integer `rowIndex`, return the `rowIndexth` (**0-indexed**) row of the **Pascal's triangle**.

In **Pascal's triangle**, each number is the sum of the two numbers directly above it as shown:

![https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)

**Example 1:**

```
Input: rowIndex = 3
Output: [1,3,3,1]

```

**Example 2:**

```
Input: rowIndex = 0
Output: [1]

```

**Example 3:**

```
Input: rowIndex = 1
Output: [1,1]

```

**Constraints:**

- `0 <= rowIndex <= 33`

**Follow up:** Could you optimize your algorithm to use only `O(rowIndex)` extra space?

Solution:

```cpp
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        if(rowIndex<0) return vector<int>();
        if(rowIndex==0) return vector<int>{1};
        
        vector<int> res(rowIndex+1,0);
        for(int k=1;k<=rowIndex;k++){
            res[0]=res[k]=1;
            for(int i=k-1;i>=1;i--){
                res[i]=res[i-1]+res[i];
            }
        }
        return res;
    }
};
```