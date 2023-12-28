# [lint] Find Peak Element II

#: 390
Difficult: Hard
Tags: BS_Idx
link: https://www.lintcode.com/problem/find-peak-element-ii/
Priority: Low
Status: More Solution
Created time: July 8, 2022 12:32 PM
Last edited time: October 12, 2023 2:57 PM
practicedTimes: 1
source: jiuzhang, 算初BS, 算高BS&SwipeLine

Given an integer matrix `A` which has the following features :

- The numbers in adjacent positions are different.
- The matrix has `n` rows and `m` columns.
- For all `i < n`, `A[i][0] < A[i][1] && A[i][m - 2] > A[i][m - 1]`.
- For all `j < m`, `A[0][j] < A[1][j] && A[n - 2][j] > A[n - 1][j]`

We define a position `[i, j]` is a peak if:

```
 A[i][j] > A[i + 1][j] && A[i][j] > A[i - 1][j] &&
 A[i][j] > A[i][j + 1] && A[i][j] > A[i][j - 1]

```

Find a peak element in this matrix. Return the index of the peak.

Example:
**Example 1:**

```
Input:
   [
     [1, 2, 3, 6,  5],
     [16,41,23,22, 6],
     [15,17,24,21, 7],
     [14,18,19,20,10],
     [13,14,11,10, 9]
   ]
Output: [1,1]
Explanation: [2,2] is also acceptable. The element at [1,1] is 41, greater than every element adjacent to it.

```

**Example 2:**

```
Input:
   [
     [1, 5, 3],
     [4,10, 9],
     [2, 8, 7]
   ]
Output: [1,1]
Explanation: There is only one peek.

```

Challenge
Solve it in O(n+m) time.

If you come up with an algorithm that you *thought* it is O(nlogm) or O(mlogn), can you prove it is actually O(n+m) or propose a similar but O(n+m) algorithm?

Related Questions:
find-peak-element

Tags:
Binary Search,LintCode Copyright,Matrix,Recursion

**solution 1**:   similar to [search-a-2d-matrix-ii/](http://www.lintcode.com/en/problem/search-a-2d-matrix-ii/), 爬行上山

**solution 2**：横切一刀，纵切一刀：[http://courses.csail.mit.edu/6.006/spring11/lectures/lec02.pdf](http://courses.csail.mit.edu/6.006/spring11/lectures/lec02.pdf)

我的思路：

1. 爬山法：一路往最高的地方爬 O(num of all cells)
2. 有什么重复计算，类似上一道题
3. 找第（k,j），发现它是否可以去掉没用的部分，也就是就是二分。
4. 不好分割，因为假设往最高的相邻格子走，也可以拐回别的区域
5. 加constraints，如果第k行找最大值，就相当于造了墙，堵住出不去了，就可以二分了，
6. 然后进一步四分

思考：

加constraints很重要，就像是maximal-rectangle这道题。

```cpp
class Solution {
public:
    /*
     * @param A: An integer matrix
     * @return: The index of the peak
     */
    vector<int> d{1,0,-1,0,1};
    vector<int> findPeakII(vector<vector<int>> &A) {
        // write your code here
        if(A.size()<3 || A[0].size()<3) return {-1,-1};
        int i=1,j=1;
        while(1){
            vector<int> next = findHigh(A,i,j);
            if(i==next[0] && j==next[1]){
                return vector<int>{i,j};
            }
            i = next[0], j = next[1];
        }
        return vector<int>{-1,-1};
    }
    vector<int> findHigh(vector<vector<int>> &A, int i, int j){
        for(int k=0;k<4;k++){
            int x=i+d[k], y=j+d[k+1];
            if(A[i][j]<A[x][y]) return vector<int>{x,y};
        }
        return vector<int>{i,j};
    }
};
```