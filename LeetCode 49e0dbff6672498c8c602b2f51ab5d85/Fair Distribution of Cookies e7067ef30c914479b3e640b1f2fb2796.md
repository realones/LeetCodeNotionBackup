# Fair Distribution of Cookies

#: 2305
Difficult: Medium
Tags: dp-K_Ranges
link: https://leetcode.com/problems/fair-distribution-of-cookies/
Priority: High
Status: More Solution, No Idea At First, toSummarize
Created time: June 30, 2023 5:19 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily
related: Find Minimum Time to Finish All Jobs (Find%20Minimum%20Time%20to%20Finish%20All%20Jobs%20b2d99b7f76c0452fb9cf90a59c8171b2.md)

You are given an integer array `cookies`, where `cookies[i]` denotes the number of cookies in the `ith` bag. You are also given an integer `k` that denotes the number of children to distribute **all** the bags of cookies to. All the cookies in the same bag must go to the same child and cannot be split up.

The **unfairness** of a distribution is defined as the **maximum** **total** cookies obtained by a single child in the distribution.

Return *the **minimum** unfairness of all distributions*.

**Example 1:**

```
Input: cookies = [8,15,10,20,8], k = 2
Output: 31
Explanation: One optimal distribution is [8,15,8] and [10,20]
- The 1st child receives [8,15,8] which has a total of 8 + 15 + 8 = 31 cookies.
- The 2nd child receives [10,20] which has a total of 10 + 20 = 30 cookies.
The unfairness of the distribution is max(31,30) = 31.
It can be shown that there is no distribution with an unfairness less than 31.

```

**Example 2:**

```
Input: cookies = [6,1,3,2,2,4,1,2], k = 3
Output: 7
Explanation: One optimal distribution is [6,1], [3,2,2], and [4,1,2]
- The 1st child receives [6,1] which has a total of 6 + 1 = 7 cookies.
- The 2nd child receives [3,2,2] which has a total of 3 + 2 + 2 = 7 cookies.
- The 3rd child receives [4,1,2] which has a total of 4 + 1 + 2 = 7 cookies.
The unfairness of the distribution is max(7,7,7) = 7.
It can be shown that there is no distribution with an unfairness less than 7.

```

**Constraints:**

- `2 <= cookies.length <= 8`
- `1 <= cookies[i] <= 105`
- `2 <= k <= cookies.length`

===================================

todo: summarize

1. divide a set to k groups
2. recur and itera for backtracking

Ideas:

divide a set to k groups → dp_k_ranges? backtracking?

================================

Solution 1 - DFS_backtracking(cookie) :

backtracking to give cookie one by one to any child

my first thought is backtracking on each child taking any subset of remaining cookies, but didn’t consider the final action: which is give ONE cookie to ONE child, so not able to figure out how to do it.

each cookie spread to a child [solution 1]

vs

each child take a subset of cookies

T: O(n^k)

there are duplicates, since children A take 5 cookies in total has no difference with B taking 5 cookies.

```cpp
class Solution {
public:
    int distributeCookies(vector<int>& cookies, int k) {
        vector<int> children(k,0);
        return helper(cookies,children,0);
    }

    int helper(vector<int>& cookies, vector<int>& children, int i){
        if(i==cookies.size()){
            return *max_element(children.begin(), children.end());
        }
        int res=INT_MAX;
        for(auto& child: children){
            child+=cookies[i];
            res=min(res,helper(cookies, children, i+1));
            child-=cookies[i];
        }
        return res;
    }

};

/*
backtracking:
cookies: 2 3 1 2 5
visited
children: 0 0 0 0

helper:
    if no more cookie, return maxEle
    for cur cookie
        for each child
            give to child
            helper(nxtCookie)
            get from child
*/
```

Solution 2: [fastest solution]

BS 剪枝 + DFS_backtracking (cookie)

```cpp
class Solution {
public:
    int distributeCookies(vector<int>& cookies, int k) {
        int total=accumulate(cookies.begin(), cookies.end(),0);
        //optional, large to small, to trigger cutting branch faster
        sort(cookies.begin(), cookies.end(),greater<>());

        int l=0,r=total;
        while(l<r){
            int m=l+(r-l)/2;
            vector<int> children(k,0);
            if(!valid(cookies, children, 0, m)) {l=m+1;}//if not qualify
            else {r=m;}
        }
        //find element
        return l;
    }

    //exist solution with maximum total of single child <= limit    
    int valid(vector<int>& cookies, vector<int>& children, int i, int limit){
        if(i==cookies.size()){ return true; }
        int flag=0;//optional, cutting branch
        for(auto& child: children){
            if(child+cookies[i]>limit) continue;
            if(child==0){
                if(flag) continue;
                flag=1;
            }
            child+=cookies[i];
            if(valid(cookies, children, i+1, limit)) return true;
            child-=cookies[i];
        }
        return false;
    }

};

/*
backtracking+BS(剪枝):
cookies: 2 3 1 2 5
visited
children: 0 0 0 0

helper:
    if no more cookie, return maxEle
    for cur cookie
        for each child
            give to child
            helper(nxtCookie)
            get from child
*
```

Solution 3:

BS + DFS (person)

each child take a subset of cookies

a take 1100110011

b take 001100000

c take …

```cpp
class Solution {
public:
    int distributeCookies(vector<int>& cookies, int k) {
        int total=accumulate(cookies.begin(), cookies.end(),0);
        //optional, large to small, to trigger cutting branch faster
        sort(cookies.begin(), cookies.end(),greater<>());

        int n=cookies.size();
        int l=0,r=total;
        while(l<r){
            int m=l+(r-l)/2;
            vector<int> children(k,0);
            if(!valid(cookies, k, 0, m, (1<<n)-1)) {l=m+1;}//if not qualify
            else {r=m;}
        }
        //find element
        return l;
    }

    //exist solution with maximum total of single child <= limit    
    int valid(vector<int>& cookies, int k, int curPerson, int limit, int state){
        if(curPerson==k){
            return state==0;
        }

        for(int subset=state; subset>0; subset=(subset-1)&state){
            int sum = getSum(cookies,subset);
            if(sum>limit) continue;
            if(valid(cookies, k, curPerson+1, limit, state-subset)) return true;
        }
        return false;
    }
    
    int getSum(vector<int>& cookies, int state){
        int res=0;
        for(int i=0; i<cookies.size(); i++){
            if((state>>i)&1){
                res+=cookies[i];
            }
        }
        return res;
    }
};

/*
dfs(people) + bs
*/
```