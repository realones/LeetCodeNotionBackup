# Burst Balloons

#: 312
Difficult: Hard
Tags: Array, DP, dp-Pyr
link: https://leetcode.com/problems/burst-balloons/
Priority: Low
Created time: September 10, 2022 11:20 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, jiuzhang, wp动归套路, 算高DP下

You are given `n` balloons, indexed from `0` to `n - 1`. Each balloon is painted with a number on it represented by an array `nums`. You are asked to burst all the balloons.

If you burst the `ith` balloon, you will get `nums[i - 1] * nums[i] * nums[i + 1]` coins. If `i - 1` or `i + 1` goes out of bounds of the array, then treat it as if there is a balloon with a `1` painted on it.

Return *the maximum coins you can collect by bursting the balloons wisely*.

**Example 1:**

```
Input: nums = [3,1,5,8]
Output: 167
Explanation:
nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] --> []
coins =  3*1*5    +   3*5*8   +  1*3*8  + 1*8*1 = 167
```

**Example 2:**

```
Input: nums = [1,5]
Output: 10

```

**Constraints:**

- `n == nums.length`
- `1 <= n <= 300`
- `0 <= nums[i] <= 100`

- 死胡同:容易想到的一个思路从小往大，枚举第一次在哪吹爆气球?
- 记忆化搜索的思路，从大到小，先考虑最后的0-n-1 合并的总价值
- State:
• dp[i][j] 表示把第i到第j个气球打爆的最大价值
- Function:
• 对于所有k属于{i,j}, 表示第k号气球最后打爆。
     • midValue = arr[i-1] * arr[k] * arr[j+1];
     • dp[i][j] = max(dp[i][k-1]+dp[k+1][j]+midvalue)
- • Intialize:
• for each i
     • dp[i][i] = arr[i-1] * arr[i] * arr[i+1];
- Answer:
• dp[0][n-1]

Solution pry:

```jsx
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        int n=nums.size();
        nums.insert(nums.begin(),1);
        nums.push_back(1);
        //idx: x 1...n x
        
        vector<vector<int>> dp(n+2,vector<int>(n+2,0));
        for(int i=1; i<=n; i++){
            dp[i][i]=nums[i-1]*nums[i]*nums[i+1];
        }
        
        for(int len=2;len<=n;len++){
            for(int l=1,r;r=l+len-1,r<=n;l++){
                for(int k=l;k<=r;k++){
                    int tmp=dp[l][k-1]+ nums[l-1]*nums[k]*nums[r+1] +dp[k+1][r];
                    dp[l][r]=max(dp[l][r], tmp);
                }
            }
        }
        
        return dp[1][n];
    }
};

/*
nums[i-1]*nums[i]*nums[i+1]
k: last killed
dp[i][j] max= dp[i][k-1] + s[i-1]*s[k]*[j+1] + dp[k+1][j]
*/
```

Solution top down:

```cpp
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        int n=nums.size(); if(n==0) {return 0;}
        vector<vector<int>> f(n,vector<int>(n,0));
        //f(k,k)=[k-1][k][k+1] - not required
        //for(int i=0;i<n;i++){
        //    f[i][i]=getNum(nums,i-1) * getNum(nums,i) * getNum(nums,i+1);
        //}
        
        //return f0,n-1
        return helper(nums,f,0,n-1);
    }
    
    int helper(vector<int>& nums, vector<vector<int>>& f, int start, int end){
        //f(i,j) = find k that max (i...j)
        //      [i-1]*[k]*[j+1] +
        //      f(i,k-1) + f(k+1,j);//if f invalid return 0
        if(start>end){return 0;}
        if(f[start][end]) return f[start][end];
        int res=0;
        for(int k=start;k<=end;k++){
            int l=helper(nums,f,start,k-1);
            int r=helper(nums,f,k+1,end);
            int m=getNum(nums,start-1)*getNum(nums,k)*getNum(nums,end+1);
            int tmp=l+m+r;
            res=max(res,tmp);
        }
        f[start][end]=res;
        return res;
    }
    
    int getNum(vector<int>& nums,int i){
        if(i<0 || i>nums.size()-1) return 1;
        return nums[i];
    }
};
```

```cpp
/*
0 1 2 k-1, k , k+1... n-1
recursive w/ dp

f(i,j) max money for range i j
f(i,j)=f(i,k-1)+nums[l]*nums[m]*nums[r]+f(k+1,j)

init f(i,i) 
return f(0, n-1)

*/
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        int n=nums.size(); if(n==0) return 0;
        vector<vector<int>> dp(n,vector<int>(n,-1));
        return helper(nums,dp,0,n-1,1,1);
    }
    
    int helper(vector<int> &nums, vector<vector<int>>& dp, int start, int end, int lval, int rval){
        if(start>end) return 0;
        if(dp[start][end]!=-1) return dp[start][end];
        int res=0;
        for(int i=start;i<=end;i++){
            //left, m, right
            int l=helper(nums,dp,start,i-1,lval,nums[i]);
            int m=lval*nums[i]*rval;
            int r=helper(nums,dp,i+1,end,nums[i],rval);
            int tmp=l+m+r;
            res=max(res,tmp);
        }
        
        dp[start][end]=res;
        return dp[start][end];
    }
};
```