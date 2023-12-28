# Palindrome Removal

#: 1246
Difficult: Hard
Tags: dp-Pyr
link: https://leetcode.com/problems/palindrome-removal/
Priority: High
Status: Dig More
Created time: December 18, 2022 11:48 PM
Last edited time: October 12, 2023 2:56 PM
source: wp动归套路

You are given an integer array `arr`.

In one move, you can select a **palindromic** subarray `arr[i], arr[i + 1], ..., arr[j]` where `i <= j`, and remove that subarray from the given array. Note that after removing a subarray, the elements on the left and on the right of that subarray move to fill the gap left by the removal.

Return *the minimum number of moves needed to remove all numbers from the array*.

**Example 1:**

```
Input: arr = [1,2]
Output: 2

```

**Example 2:**

```
Input: arr = [1,3,4,1,5]
Output: 3
Explanation:Remove [4] then remove [1,3,1] then remove [5].

```

**Constraints:**

- `1 <= arr.length <= 100`
- `1 <= arr[i] <= 20`

**Solution**:

题意描述：给一个字符串s，每次删除其中的一个回文substring，问多少次删完？

本题并没有特别明显的套路特征。第二类区间型DP应该是穷尽其他想法之后的兜底方案。我们定义dp[i:j]表示对于s[i:j]最少需要删几次。接下来考虑，如何将这个大区间转化为小区间呢？如果没有其他明显的特征的话，一个常见的处理方法就是看最后一个元素s[j]。

因为无论怎么设计方案，s[j]都可以是最后一个被删除的。我们就想s[j]会和谁一起删除？必然是找相同的字符。因此我们在[i:j]中寻找一个s[k]==s[j]，那么原来的大区间就分化为了两部分：dp[i][k-1]+dp[k][j]。而对于后者，我们发现s[k]和s[j]并不占用操作，而是可以和删除dp[k+1][j-1]的时候一起消除（回文加了一层而已）。

因此状态转移方程是：

```
        for (int len = 1; len <= N; len++)
            for (int i = 1; i+len-1<=N; i++)
            {
                int j = i+len-1;
                dp[i][j] = dp[i][j-1]+1;
                for (int k=i; k<j; k++)
                {
                    if (arr[k]==arr[j])
                        dp[i][j] = min(dp[i][j], dp[i][k-1]+max(1,dp[k+1][j-1]));
                }
            }
```

特别注意，删除s[k]和s[j]并不是永远都不占用操作数，如果当dp[k+1][j-1]本身是0的时候，我们还是需要将s[k]和s[j]当作一对紧挨着的回文数删去的。

```jsx
class Solution {
public:
    int minimumMoves(vector<int>& arr) 
    {
        int n=arr.size();
        arr.insert(arr.begin(),0);
        vector<vector<int>> dp(n+2,vector<int>(n+2,0));
        for(int i=1;i<=n;i++){dp[i][i]=1;}//this line not required
        for(int len=2;len<=n;len++){
            for(int l=1,r;r=l+len-1,r<=n;l++){
                dp[l][r]=INT_MAX;
                for(int x=l;x<=r;x++){
                    int tmp=
                            dp[l][x-1] + (
                            arr[x]==arr[r]?
                                max(1,dp[x+1][r-1]) // e.g. [AA], should be 1 but not 0
                                :
                                dp[x][r]
                            );
                    dp[l][r]=min(dp[l][r],tmp);
                }
            }
        }
        return dp[1][n];
    }
};

/*
dp-pyr
dpij: min steps for range [i..j]
since can remove palindrome from inside or from side,each way we can remove edge in the end, so consider rm [x..j] in the end
dpij min= dpix-1   (x in [i..j])
          + 
          if([x]==[j])
            max(1,dpx+1j-1) //for situation e.g. [AA]
          el
            dpxj
          
*/
```