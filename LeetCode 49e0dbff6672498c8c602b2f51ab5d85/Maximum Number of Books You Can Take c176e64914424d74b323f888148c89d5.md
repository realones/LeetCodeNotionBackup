# Maximum Number of Books You Can Take

#: 2355
Difficult: Hard
Tags: Array, DP, monotonic, stack
link: https://leetcode.com/problems/maximum-number-of-books-you-can-take
Priority: High
Status: Dig More, No Idea At First
Created time: November 15, 2023 4:29 PM
Last edited time: November 17, 2023 1:26 AM
practicedTimes: 1
source: AmazonFreq

You are given a **0-indexed** integer array `books` of length `n` where `books[i]` denotes the number of books on the `ith` shelf of a bookshelf.

You are going to take books from a **contiguous** section of the bookshelf spanning from `l` to `r` where `0 <= l <= r < n`. For each index `i` in the range `l <= i < r`, you must take **strictly fewer** books from shelf `i` than shelf `i + 1`.

Return *the **maximum** number of books you can take from the bookshelf.*

**Example 1:**

```
Input: books = [8,5,2,7,9]
Output: 19
Explanation:
- Take 1 book from shelf 1.
- Take 2 books from shelf 2.
- Take 7 books from shelf 3.
- Take 9 books from shelf 4.
You have taken 19 books, so return 19.
It can be proven that 19 is the maximum number of books you can take.

```

**Example 2:**

```
Input: books = [7,0,3,4,5]
Output: 12
Explanation:
- Take 3 books from shelf 2.
- Take 4 books from shelf 3.
- Take 5 books from shelf 4.
You have taken 12 books so return 12.
It can be proven that 12 is the maximum number of books you can take.

```

**Example 3:**

```
Input: books = [8,2,3,7,3,4,0,1,4,3]
Output: 13
Explanation:
- Take 1 book from shelf 0.
- Take 2 books from shelf 1.
- Take 3 books from shelf 2.
- Take 7 books from shelf 3.
You have taken 13 books so return 13.
It can be proven that 13 is the maximum number of books you can take.

```

**Constraints:**

- `1 <= books.length <= 105`
- `0 <= books[i] <= 105`

comment: difficult to figure out the algorithm, checked wisdompeak

Solution:

```cpp
using ll=long long;

class Solution {
public:
    long long maximumBooks(vector<int>& nums) {
        int n=nums.size();
        //process pre[i], to find which j the i is relied on
        stack<int> stk;
        vector<int> pre(n,-1);
        for(int i=0; i<n; i++){
            while(!stk.empty() && nums[stk.top()]>=nums[i]-(i-stk.top())) stk.pop();
            if(!stk.empty()) pre[i]=stk.top();
            stk.push(i);
        }
        //dp process
        vector<ll> dp(n,-1);
        ll res=0;
        for(int i=0; i<n; i++){
            if(pre[i]==-1){
                int d = min(i+1, nums[i]);
                dp[i] = ((ll)nums[i] + nums[i] - d + 1) * d /2;     
            }
            else{
                int j=pre[i];
                int d=i-j+1-1;//from j+1
                dp[i] = dp[j] + ((ll)nums[i] + nums[i] - d + 1) * d /2;
            }
            res=max(res,dp[i]);
        }
        return res;
    }
};

/*
nums[i]: num of nums of ith shelf
take nums from contiguous section of shelves [i,j]
must take strictly fewer nums from shelf i than shelf i + 1
return: max nums to take
================================
len: 1~1e5
val: 0~1e5
================================
from ans:
dp[i]: for nums[0~i-1], with i taken, max sum to take
dp[i] = find 1st j that nums[j]<nums[i]-(i-j)
      = dp[j] + sum(x,x+1,x+2,...,nums[i]) with len(i-j+1) in range (j,i)
      = dp[j] + (i-j+1) / 2 * (nums[i]+nums[i]-i+j);
      if not found j (j==-1), dp[i]= (i+1)/2*(nums[i]+nums[i]-i);
init: corner case as above when j not found
return: max of dp[any]
================================
sub question: how to find 1st j that dp[j]<dp[i] - (i-j) 
    mono stk:
    for coming cur:
        if top qualified to dec (i-j), nums[j]>=nums[i]-(i-j), pop
        now top is the unqualified one: nums[j]<nums[i]-(i-j)
        we got dp[i] = dp[j] + sum described above
*/
```