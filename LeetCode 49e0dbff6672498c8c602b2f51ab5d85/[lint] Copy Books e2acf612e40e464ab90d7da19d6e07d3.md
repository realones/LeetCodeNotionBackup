# [lint] Copy Books

#: 437
Difficult: Medium
Tags: BS_Val
link: https://www.lintcode.com/problem/437/
Priority: Low
Created time: July 8, 2022 4:26 PM
Last edited time: October 12, 2023 2:57 PM
practicedTimes: 1
source: jiuzhang, 算初BS, 算高BS&SwipeLine

**Description**

Given `n` books and the `i-th` book has `pages[i]` pages. There are `k` persons to copy these books.

These books list in a row and each person can claim a continous range of books. For example, one copier can copy the books from `i-th` to `j-th` continously, but he can not copy the 1st book, 2nd book and 4th book (without 3rd book).

They start copying books at the same time and they all cost 1 minute to copy 1 page of a book. What's the best strategy to assign books so that the slowest copier can finish at earliest time?

Return the shortest time that the slowest copier spends.

---

The sum of book pages is less than or equal to 2147483647

**Example**

**Example 1:**

```
Input: pages = [3, 2, 4], k = 2
Output: 5
Explanation:
    First person spends 5 minutes to copy book 1 and book 2.
    Second person spends 4 minutes to copy book 3.

```

**Example 2:**

```
Input: pages = [3, 2, 4], k = 3
Output: 4
Explanation: Each person copies one of the books.

```

**Challenge**

O(nk) time

思考res的范围

- **other solution exists**

逆向思路：

最终解有范围，可以对其二分

判断当前解是否valid，用贪心法得出最少的人数，对比是否小于总人数。

建模
11111  111  11  111111111  11  111111  1  111
divide to k pieces
the largest piece as small as possible, to make the division more average, return the size of the largest piece
想不出公式来，还要求可用的最优值，直接val二分了

```cpp
#include <numeric>
class Solution {
public:
    /**
     * @param pages: an array of integers
     * @param k: An integer
     * @return: an integer
     */
    int copyBooks(vector<int> &nums, int k) {
        // write your code here
        int n=nums.size(); if(n==0) {return 0;}
        if(k<1) return -1;
        int max=*max_element(nums.begin(),nums.end());
        int sum=std::accumulate(nums.begin(),nums.end(),0);
        int l=max, r=sum;
        while(l<r){
            int m=l+(r-l)/2;
            if(!valid(m,nums,k)) l=m+1;
            else r=m;
        }
        return l;
    }

    bool valid(int time, vector<int>& nums, int k){
        int curSum=0;
        int expectedPeople=1;
        for(auto num: nums){
            if(curSum+num > time){
                expectedPeople++;
                curSum=0;
            }
            curSum += num;
        }
        return expectedPeople <= k;
    }
};
```