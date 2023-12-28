# [lint] Kth Smallest Sum In Two Sorted Arrays

#: 465
Difficult: Hard
Tags: BS_Idx, PQ
link: https://www.lintcode.com/problem/kth-smallest-sum-in-two-sorted-arrays
Priority: Low
Status: More Solution, No Idea At First
Created time: August 22, 2022 1:08 AM
Last edited time: October 22, 2023 2:07 PM
practicedTimes: 1
source: jiuzhang, 算高TwoPointers&PQ

Given two integer arrays sorted in ascending order and an integer k. Define *sum = a + b*, where *a* is an element from the first array and *b* is an element from the second one. Find the *k*th smallest sum out of all possible sums.

Example:
**Example 1**

```
Input:
a = [1, 7, 11]
b = [2, 4, 6]
k = 3
Output: 7
Explanation: The sums are [3, 5, 7, 9, 11, 13, 13, 15, 17] and the 3th is 7.

```

**Example 2**

```
Input:
a = [1, 7, 11]
b = [2, 4, 6]
k = 4
Output: 9
Explanation: The sums are [3, 5, 7, 9, 11, 13, 13, 15, 17] and the 4th is 9.

```

**Example 3**

```
Input:
a = [1, 7, 11]
b = [2, 4, 6]
k = 8
Output: 15
Explanation: The sums are [3, 5, 7, 9, 11, 13, 13, 15, 17] and the 8th is 15.

```

Challenge
Do it in either of the following time complexity:

1. O(k log min(n, m, k)). where n is the size of A, and m is the size of B.
2. O( (m + n) log maxValue). where maxValue is the max number in A and B.

九章用的PQ，其实用BS更好

```cpp
class Node {
public:
    int value, x, y;
    Node(int _v, int _x, int _y) {
        value = _v;
        x = _x;
        y = _y;
    }
    bool operator < (const Node& obj) const {
        return value > obj.value;
    }
};

class Solution {
public:

    /*
     * @param A: an integer arrays sorted in ascending order
     * @param B: an integer arrays sorted in ascending order
     * @param k: An integer
     * @return: An integer
     */
    int kthSmallestSum(vector<int> A, vector<int> B, int k) {
        // write your code here
        //similar to ugly number 2 || kth-smallest-number-in-sorted-matrix
        int m = A.size();
        int n = B.size();
        priority_queue<Node> pq;
        pq.emplace(A[0] + B[0], 0, 0);
        vector<vector<bool>> visit(m,vector<bool>(n,false));

        vector<int> dx{0,1};
        vector<int> dy{1,0};

        for(int i=0;i<k-1;i++){
            Node cur=pq.top(); pq.pop();
            int x=cur.x, y=cur.y;
            for(int k=0;k<2;k++){
                int nx=x+dx[k],ny=y+dy[k];
                if(nx>=m || ny>=n || visit[nx][ny]) continue;
                visit[nx][ny]=true;
                int nval=A[nx]+B[ny];
                pq.emplace(nval,nx,ny);
            }
        }
        return pq.top().value;
    }
};
```