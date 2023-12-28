# Filling Bookcase Shelves

#: 1105
Difficult: Medium
Tags: DP, dp-TimeSeqFromLastX
link: https://leetcode.com/problems/filling-bookcase-shelves/
Priority: High
Status: HardToImplement, More Solution
Created time: October 30, 2022 8:39 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, wp动归套路

You are given an array `books` where `books[i] = [thicknessi, heighti]` indicates the thickness and height of the `ith` book. You are also given an integer `shelfWidth`.

We want to place these books in order onto bookcase shelves that have a total width `shelfWidth`.

We choose some of the books to place on this shelf such that the sum of their thickness is less than or equal to `shelfWidth`, then build another level of the shelf of the bookcase so that the total height of the bookcase has increased by the maximum height of the books we just put down. We repeat this process until there are no more books to place.

Note that at each step of the above process, the order of the books we place is the same order as the given sequence of books.

- For example, if we have an ordered list of `5` books, we might place the first and second book onto the first shelf, the third book on the second shelf, and the fourth and fifth book on the last shelf.

Return *the minimum possible height that the total bookshelf can be after placing shelves in this manner*.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/06/24/shelves.png](https://assets.leetcode.com/uploads/2019/06/24/shelves.png)

```
Input: books = [[1,1],[2,3],[2,3],[1,1],[1,1],[1,1],[1,2]], shelf_width = 4
Output: 6
Explanation:
The sum of the heights of the 3 shelves is 1 + 3 + 2 = 6.
Notice that book number 2 does not have to be on the first shelf.

```

**Example 2:**

```
Input: books = [[1,3],[2,4],[3,2]], shelfWidth = 6
Output: 4

```

**Constraints:**

- `1 <= books.length <= 1000`
- `1 <= thicknessi <= shelfWidth <= 1000`
- `1 <= heighti <= 1000`

Solution 1:

```cpp
class Solution {
public:
    int minHeightShelves(vector<vector<int>>& books, int shelfWidth) {
        int n=books.size(); if(n==0) return 0;
        vector<int> f(n,INT_MAX);
        
        /*
        fk = min of 
                    h1+h2
             h1 = f(x-1)
             h2 = max of height(x..k) x..k in one row <== w(x+..+k)<=W
        */
        for(int k=0;k<n;k++){
            int curW=0, curH=0;
            for(int x=k;x>=0 && curW+books[x][0] <= shelfWidth;x--){
                curW+=books[x][0];
                curH=max(curH,books[x][1]);
                int h1= x-1>=0 ? f[x-1]:0;
                f[k]=min(f[k],h1+curH);
            }
        }
        
        return f.back();
    }
};
```

Solution: top-down

```cpp
class Solution {
public:
    int minHeightShelves(vector<vector<int>>& books, int shelfWidth) {
        int n=books.size(); if(n==0) {return 0;}
        vector<int> dp(n,0);//k~n-1
        return helper(books, shelfWidth, dp, 0);
    }
    
    int helper(vector<vector<int>>& books, int shelfWidth, vector<int>&dp, int i) {
        if(i==books.size()) return 0;
        if(dp[i]) return dp[i];
        int res=INT_MAX;
        int curH=0;
        int curW=0;
        for(int j=i;j<books.size() && curW+books[j][0]<=shelfWidth; j++){
            curH=max(curH,books[j][1]);
            curW+=books[j][0];
            res=min(res,curH+helper(books, shelfWidth, dp, j+1));
        }
        
        dp[i]=res;
        return res;
    }
};
```