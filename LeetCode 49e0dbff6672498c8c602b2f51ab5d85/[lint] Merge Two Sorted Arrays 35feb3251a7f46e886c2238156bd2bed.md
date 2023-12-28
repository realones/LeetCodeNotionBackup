# [lint] Merge Two Sorted Arrays

#: 6
Difficult: Easy
Tags: Array, Merge
link: https://www.lintcode.com/problem/6/
Priority: Pass
Created time: August 3, 2022 1:15 PM
Last edited time: October 16, 2023 4:59 PM
practicedTimes: 1
source: jiuzhang, 算初LinkedList&Array

**Description**

Merge two given sorted ascending integer array *A* and *B* into a new sorted integer array.

---

**Example**

**Example 1:**

Input:

```
A = [1]
B = [1]

```

Output:

```
[1,1]

```

Explanation:

return array merged.

**Example 2:**

Input:

```
A = [1,2,3,4]
B = [2,4,5,6]

```

Output:

```
[1,2,2,3,4,4,5,6]

```

**Challenge**

How can you optimize your algorithm if one array is very large and the other is very small?

```cpp
class Solution {
public:
    /*
     * @param A: sorted integer array A
     * @param B: sorted integer array B
     * @return: A new sorted integer array
     */
    vector<int> mergeSortedArray(vector<int> &A, vector<int> &B) {
        vector<int> res;
        int i=0,j=0;
        while(i<A.size() && j<B.size()){
            if(A[i]<B[j]){res.push_back(A[i]);i++;}
            else{res.push_back(B[j]);j++;}
        }
        while(i<A.size()){res.push_back(A[i]);i++;}
        while(j<B.size()){res.push_back(B[j]);j++;}
        
        return res;
    }
};
```