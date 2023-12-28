# Kth Smallest Number in Multiplication Table

#: 668
Difficult: Hard
Tags: BS_Val
link: https://leetcode.com/problems/kth-smallest-number-in-multiplication-table/
Priority: Medium
Created time: June 27, 2023 11:33 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Kth Smallest Element in a Sorted Matrix (Kth%20Smallest%20Element%20in%20a%20Sorted%20Matrix%20a26a785e28ae4e428ca85c4bafa8906c.md)

Nearly everyone has used the [Multiplication Table](https://en.wikipedia.org/wiki/Multiplication_table). The multiplication table of size `m x n` is an integer matrix `mat` where `mat[i][j] == i * j` (**1-indexed**).

Given three integers `m`, `n`, and `k`, return *the* `kth` *smallest element in the* `m x n` *multiplication table*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/05/02/multtable1-grid.jpg](https://assets.leetcode.com/uploads/2021/05/02/multtable1-grid.jpg)

```
Input: m = 3, n = 3, k = 5
Output: 3
Explanation: The 5th smallest number is 3.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/05/02/multtable2-grid.jpg](https://assets.leetcode.com/uploads/2021/05/02/multtable2-grid.jpg)

```
Input: m = 2, n = 3, k = 6
Output: 6
Explanation: The 6th smallest number is 6.

```

**Constraints:**

- `1 <= m, n <= 3 * 104`
- `1 <= k <= m * n`

```cpp
class Solution {
public:
    int findKthNumber(int m, int n, int k) {
        int l=1,r=m*n;
        while(l<r){
            int mid=l+(r-l)/2;
            if(!valid(m,n,k,mid)) {l=mid+1;}//if not qualify
            else {r=mid;}
        }
        return l;
    }

    bool valid(int m, int n, int k, int maxVal){
        int cnt=0;
        int j=n;
        for(int i=1;i<=m;i++){
            for(;j>0 && i*j>maxVal;j--);
            cnt+=j;
        }
        return cnt>=k;
    }
};

/*
mat 1 based
mat[i][j] == i * j (1-indexed).
return the kth smallest element
1 <= m, n <= 3 * 1e4
================================
pq: klog(k) TLE
================================
bs on val:
find first element with k element <= it
NNNNNYYYYYYY
log(m*n) * validation(n)
*/
```