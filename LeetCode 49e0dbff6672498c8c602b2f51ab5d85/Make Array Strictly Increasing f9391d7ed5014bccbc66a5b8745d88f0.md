# Make Array Strictly Increasing

#: 1187
Difficult: Hard
Tags: Array, BS_lower_upper_bound, Sort, dp-TimeSeqFromLastOne-Power
link: https://leetcode.com/problems/make-array-strictly-increasing/
Priority: High
Status: HardToImplement, More Solution
Created time: June 17, 2023 6:01 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, leetcode_daily

Given two integer arrays `arr1` and `arr2`, return the minimum number of operations (possibly zero) needed to make `arr1` strictly increasing.

In one operation, you can choose two indices `0 <= i < arr1.length` and `0 <= j < arr2.length` and do the assignment `arr1[i] = arr2[j]`.

If there is no way to make `arr1` strictly increasing, return `-1`.

**Example 1:**

```
Input: arr1 = [1,5,3,6,7], arr2 = [1,3,2,4]
Output: 1
Explanation: Replace5 with2, thenarr1 = [1, 2, 3, 6, 7].

```

**Example 2:**

```
Input: arr1 = [1,5,3,6,7], arr2 = [4,3,1]
Output: 2
Explanation: Replace5 with3 and then replace3 with4.arr1 = [1, 3, 4, 6, 7].

```

**Example 3:**

```
Input: arr1 = [1,5,3,6,7], arr2 = [1,6,3,3]
Output: -1
Explanation: You can't makearr1 strictly increasing.
```

**Constraints:**

- `1 <= arr1.length, arr2.length <= 2000`
- `0 <= arr1[i], arr2[i] <= 10^9`

```cpp
class Solution {
public:
    int makeArrayIncreasing(vector<int>& arr1, vector<int>& arr2) {
       int n=arr1.size();
       sort(arr2.begin(), arr2.end());
       arr1.insert(arr1.begin(),0);
       vector<vector<int>> dp(n+1,vector<int>(n+1,INT_MAX));//init as invalid

       dp[0][0]=INT_MIN;

       for(int i=1;i<n+1;i++){
            for(int j=0;j<=i;j++){                
                if(dp[i-1][j]<arr1[i]){
                    dp[i][j]=min(dp[i][j], arr1[i]);
                }
                if(j>=1){
                    auto iter = upper_bound(arr2.begin(), arr2.end(), dp[i-1][j-1]);
                    if(iter!=arr2.end()){
                        dp[i][j]=min(dp[i][j], *iter);
                    }
                }
            }
        }

        for(int j=0; j<n+1; j++){
            if(dp.back()[j]!=INT_MAX) return j;
        }
        return -1;
    }
};

/*
T: 2000 * 2000 * log 2000 ?

X X X i X X X
i can be it self or upper_bound of arr[i-1] from arr2 to keep inc

dp[i][j]: arr1[0..i] strictly inc, operation j times, min val of arr[i]

dp[i][j] = 
    no switch -> arr[i]; only_if(dp[i-1][j]<arr[i])
    switch -> arr[i] = arr2.upper_bound(d[i-1][j-1])
*/
```

```cpp
class Solution {
public:
    int makeArrayIncreasing(vector<int>& arr1, vector<int>& arr2) {
        sort(arr2.begin(), arr2.end());
        int n1=arr1.size();
        int n2=arr2.size();
        vector<vector<int>> dp(n1,vector<int>(n2+1,INT_MAX));
        //init
        dp[0][0]=arr1[0];
        for(int j=1; j<=n2; j++){
            dp[0][j]=min(arr1.front(), arr2.front());
        }
        for(int i=1; i<n1; i++){
            dp[i][0]= arr1[i]>arr1[i-1]?arr1[i]:INT_MAX;
            if(dp[i][0]==INT_MAX) break;
        }
        //init other j=0
        //process
        for(int i=1; i<n1; i++){
            for(int j=1; j<=n2; j++){
                int can1= dp[i-1][j]<arr1[i]? arr1[i]: INT_MAX;
                auto itr=upper_bound(arr2.begin(), arr2.end(), dp[i-1][j-1]);
                int can2= itr!=arr2.end()? *itr : INT_MAX;
                dp[i][j]=min({dp[i][j],can1,can2});
            }
        }
        //return
        for(int j=0; j<=n2; j++){
            if(dp[n1-1][j]!=INT_MAX) return j;
        }
        return -1;
    }
};
/*
dp[idx][operationTime]: for element 0~idx i, after operation j times, what is the minVal you can get on idx i, which most benefit to minimize future operation steps
idx: 0~n1-1
operationTime: 0~n2
invalid: INT_MAX

init:
    all INT_MAX
    dp[0][0] = arr[0]
    dp[0][other] = smallest of (arr1[0],arr2[0])

dp[i][j] min= 
        dp[i-1][j]:   if(dp[i-1][j]<arr[i]) then good, use arr[i], otherwise, INT_MAX which is invalid
        dp[i-1][j-1]: next bigger one in arr2 of dp[i-1][j-1]

return
    dp[n-1][any] which not in_valid
*/
```