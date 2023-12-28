# [lint] Copy Books II

#: 438
Difficult: Hard
Tags: BS_Val
link: https://www.lintcode.com/problem/copy-books-ii/description
Priority: Pass
Status: More Solution
Created time: September 8, 2022 7:50 PM
Last edited time: October 28, 2023 5:21 PM
practicedTimes: 1
source: jiuzhang, 算高BS&SwipeLine

Given `n` books and each book has the same number of pages. There are `k` persons to copy these books and the `i-th` person needs `times[i]` minutes to copy a book.

Each person can copy any number of books and they start copying at the same time. What's the best strategy to assign books so that the job can be finished at the earliest time?

Return the shortest time.

Example:
**Example 1:**

```
Input: n = 4, times = [3, 2, 4]
Output: 4
Explanation:
   First person spends 3 minutes to copy 1 book.
   Second person spends 4 minutes to copy 2 books.
   Third person spends 4 minutes to copy 1 book.

```

**Example 2:**

```
Input: n = 4, times = [3, 2, 4, 5]
Output: 4
Explanation: Use the same strategy as example 1 and the forth person does nothing.

```

todo: dp solution to be implemented

```cpp
class Solution {
public:
    /**
     * @param n: An integer
     * @param times: an array of integers
     * @return: an integer
     */
    int copyBooksII(int n, vector<int> &times) {
        // write your code here
        if(n==0){return 0;}
        if(times.size()==0){return INT_MAX;}
        int min_time=INT_MAX, max_time=0;
        for(auto time:times){
            min_time=min(min_time,time);
            max_time=max(max_time,time);
        }
        int l=0,r=n*max_time;
        while(l<r){
            int m=l+(r-l)/2;
            if(!valid(n,times,m)) {l=m+1;}//if not qualify
            else {r=m;}
        }
        return l;
    }

    bool valid(int n, vector<int>& times, int expected_total_time){
        int canTransBooks=0;
        for(auto time: times){
            canTransBooks+=expected_total_time/time;
        }
        return canTransBooks>=n;
    }
};
```