# K-th Smallest Prime Fraction

#: 786
Difficult: Medium
Tags: Array, BS_Val, Greedy, PQ, Sort
link: https://leetcode.com/problems/k-th-smallest-prime-fraction/
Priority: High
Status: No Idea At First
Created time: June 27, 2023 11:34 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Kth Smallest Element in a Sorted Matrix (Kth%20Smallest%20Element%20in%20a%20Sorted%20Matrix%20a26a785e28ae4e428ca85c4bafa8906c.md), Find K-th Smallest Pair Distance (Find%20K-th%20Smallest%20Pair%20Distance%200447f8ad46f94f05991e0c16d880e076.md)

You are given a sorted integer array `arr` containing `1` and **prime** numbers, where all the integers of `arr` are unique. You are also given an integer `k`.

For every `i` and `j` where `0 <= i < j < arr.length`, we consider the fraction `arr[i] / arr[j]`.

Return *the* `kth` *smallest fraction considered*. Return your answer as an array of integers of size `2`, where `answer[0] == arr[i]` and `answer[1] == arr[j]`.

**Example 1:**

```
Input: arr = [1,2,3,5], k = 3
Output: [2,5]
Explanation: The fractions to be considered in sorted order are:
1/5, 1/3, 2/5, 1/2, 3/5, and 2/3.
The third fraction is 2/5.

```

**Example 2:**

```
Input: arr = [1,7], k = 1
Output: [1,7]

```

**Constraints:**

- `2 <= arr.length <= 1000`
- `1 <= arr[i] <= 3 * 104`
- `arr[0] == 1`
- `arr[i]` is a **prime** number for `i > 0`.
- All the numbers of `arr` are **unique** and sorted in **strictly increasing** order.
- `1 <= k <= arr.length * (arr.length - 1) / 2`

**Follow up:**

Can you solve the problem with better than

```
O(n2)
```

complexity?

my comment:

PQ solution 相对好想，pq可以那bs肯定也可以，都是类似于“Kth Smallest Element in a Sorted Matrix”的变化趋势。

BS怎么找pair而不是val比较难想，而且忘记加abs，导致有个test case卡了很久

Solution - PQ:

```cpp
using fij=tuple<double, int ,int>;

class Solution {
public:
    vector<int> kthSmallestPrimeFraction(vector<int>& arr, int k) {
        int n=arr.size();
        vector<vector<int>> visited(n,vector<int>(n,0));
        priority_queue<fij,vector<fij>,greater<>> pq;
        pq.emplace(arr[0]*1.0/arr[n-1],0,n-1);
        for(int t=0; t<k-1; t++){
            auto [f,i,j]=pq.top(); pq.pop();
            if(!visited[i+1][j]){
                visited[i+1][j]=1;
                pq.emplace(arr[i+1]*1.0/arr[j],i+1,j);
            }
            if(!visited[i][j-1]){
                visited[i][j-1]=1;
                pq.emplace(arr[i]*1.0/arr[j-1],i,j-1);
            }
        }
        auto [f,i,j]=pq.top();
        return {arr[i],arr[j]};
    }
};

/*
fraction is arr[i] / arr[j].
find {i,j} with kth smallest arr[i]/arr[j]
================================
pq
init 0/n-1
either i++, or j--
================================
*/
```

Solution - bs_val:

```cpp
class Solution {
public:
    double eps = 1e-12;
    int x=0,y=0;

    vector<int> kthSmallestPrimeFraction(vector<int>& arr, int k) {
        int n=arr.size();
        double l=0,r=1;
        while(r-l > eps) {
            double m = l+(r-l)/2;
            if(!valid(arr,k,m)) {l = m;}
            else {r = m;}
        }
        return {arr[x],arr[y]};
    }

    //return cnt(fra<=mid) >=k
    bool valid(vector<int>& arr, int k, double maxFra){
        int n=arr.size();
        int cnt=0;
        int j=1;
        for(int i=0; i<n-1; i++){//ingore last row
            for(; j<n && arr[i]*1.0/arr[j]>maxFra; j++);
            cnt+=n-j;
            if(i<n-1 && j<n && abs(arr[i]*1.0/arr[j]-maxFra)<=eps){  x=i;y=j; }
        }
        return cnt>=k;
    }
};

/*
================================
i/j = row/col, i<j
   --------dec-------->
   1   2   3   5   7   ^
1     1/2 1/3 1/5 1/7  |
2         2/3 2/5 2/7  dec
3             3/5 3/7  |
5                 5/7  |
7                      |
================================

k: 1~len*(len-1)/2
l:0, r:1
if just search faction val:
    kthSmallest -> cnt(fra<=mid) >=k
    NNNNNNNYYYYYYY
but ask for return pair of {i,j}:
    update {i,j} for every valid(m)
*
```