# Find K-th Smallest Pair Distance

#: 719
Difficult: Hard
Tags: 2pt_SameDir_maxWin_chkValidAftR, Array, BS_Val, Sort
link: https://leetcode.com/problems/find-k-th-smallest-pair-distance/
Priority: High
Status: More Solution
Created time: June 27, 2023 11:36 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: K-th Smallest Prime Fraction (K-th%20Smallest%20Prime%20Fraction%207c951e5c8ed7479dac2997b627722d17.md), Kth Smallest Element in a Sorted Matrix (Kth%20Smallest%20Element%20in%20a%20Sorted%20Matrix%20a26a785e28ae4e428ca85c4bafa8906c.md)

The **distance of a pair** of integers `a` and `b` is defined as the absolute difference between `a` and `b`.

Given an integer array `nums` and an integer `k`, return *the* `kth` *smallest **distance among all the pairs*** `nums[i]` *and* `nums[j]` *where* `0 <= i < j < nums.length`.

**Example 1:**

```
Input: nums = [1,3,1], k = 1
Output: 0
Explanation: Here are all the pairs:
(1,3) -> 2
(1,1) -> 0
(3,1) -> 2
Then the 1st smallest distance pair is (1,1), and its distance is 0.

```

**Example 2:**

```
Input: nums = [1,1,1], k = 2
Output: 0

```

**Example 3:**

```
Input: nums = [1,6,1], k = 3
Output: 5

```

**Constraints:**

- `n == nums.length`
- `2 <= n <= 104`
- `0 <= nums[i] <= 106`
- `1 <= k <= n * (n - 1) / 2`

my comment:

一开始看到i<j就认为seq matters，不能sort，其实因为abs所以可以sort，然后就简单了。

same with “K-th Smallest Prime Fraction”

但是bs_val也用了2pt 想了一会

todo:

好像有不用2pt的解法，或者dp解法

Solution PQ - TLE:

```cpp
using dxy=tuple<int,int,int>;
vector<int> dx{1,0};
vector<int> dy{0,-1};

class Solution {
public:
    int smallestDistancePair(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int n=nums.size();
        k=n*(n-1)/2 - k+1;
        priority_queue<dxy,vector<dxy>,less<>> pq;
        vector<vector<int>> visited(n,vector<int>(n,0));
        pq.emplace(nums.back()-nums.front(), 0, n-1);
        visited[0][n-1]=1;
        for(int i=0; i<k-1; i++){
            auto [d,x,y] = pq.top(); pq.pop();
            for(int t=0;t<2;t++){
                int nx=x+dx[t];
                int ny=y+dy[t];
                if(nx>=ny) continue;
                if(visited[nx][ny]) continue;
                visited[nx][ny]=1;
                pq.emplace(nums[ny]-nums[nx],nx,ny);
            }
        }
        return get<0>(pq.top());
    }
};

/*
kth smallest abs(i,j) i<j
    seq not matters -> sort
pq:
    init: largest must be back-front
    change question to final kthLargest with k=n*(n-1)/2 - k
    go smaller: either i(init:0)++, or j(init:n-1)--
*
```

Solution BS_val + 2pt_maxWindow:

```cpp
class Solution {
public:
    int smallestDistancePair(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int l=0, r=1e6;
        while(l<r){
            int m=l+(r-l)/2;
            if(!valid(nums,k,m)) {l=m+1;}//if not qualify
            else {r=m;}
        }
        return l;
    }

    //cnt(pair with dist <= maxDist) >=k
    bool valid(vector<int>& nums, int k, int maxDist){
        int n=nums.size();
        int cnt=0;
        //maintain max sliding window
        for(int l=0,r=1;r<n;r++){
            while(nums[r]-nums[l]>maxDist) l++;
            if(l<r) cnt+=r-l;//pair cnt end with r
        }
        return cnt>=k;
    }
}
```