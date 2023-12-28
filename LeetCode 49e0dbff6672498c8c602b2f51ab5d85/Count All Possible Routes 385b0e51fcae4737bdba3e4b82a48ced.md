# Count All Possible Routes

#: 1575
Difficult: Hard
Tags: DP
link: https://leetcode.com/problems/count-all-possible-routes/
Priority: High
Status: HardToImplement, More Solution
Created time: June 24, 2023 11:50 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

You are given an array of **distinct** positive integers locations where `locations[i]` represents the position of city `i`. You are also given integers `start`, `finish` and `fuel` representing the starting city, ending city, and the initial amount of fuel you have, respectively.

At each step, if you are at city `i`, you can pick any city `j` such that `j != i` and `0 <= j < locations.length` and move to city `j`. Moving from city `i` to city `j` reduces the amount of fuel you have by `|locations[i] - locations[j]|`. Please notice that `|x|` denotes the absolute value of `x`.

Notice that `fuel` **cannot** become negative at any point in time, and that you are **allowed** to visit any city more than once (including `start` and `finish`).

Return *the count of all possible routes from* `start` *to* `finish`. Since the answer may be too large, return it modulo `109 + 7`.

**Example 1:**

```
Input: locations = [2,3,6,8,4], start = 1, finish = 3, fuel = 5
Output: 4
Explanation: The following are all possible routes, each uses 5 units of fuel:
1 -> 3
1 -> 2 -> 3
1 -> 4 -> 3
1 -> 4 -> 2 -> 3

```

**Example 2:**

```
Input: locations = [4,3,1], start = 1, finish = 0, fuel = 6
Output: 5
Explanation: The following are all possible routes:
1 -> 0, used fuel = 1
1 -> 2 -> 0, used fuel = 5
1 -> 2 -> 1 -> 0, used fuel = 5
1 -> 0 -> 1 -> 0, used fuel = 3
1 -> 0 -> 1 -> 0 -> 1 -> 0, used fuel = 5

```

**Example 3:**

```
Input: locations = [5,2,1], start = 0, finish = 2, fuel = 3
Output: 0
Explanation: It is impossible to get from 0 to 2 using only 3 units of fuel since the shortest route needs 4 units of fuel.

```

**Constraints:**

- `2 <= locations.length <= 100`
- `1 <= locations[i] <= 109`
- All integers in `locations` are **distinct**.
- `0 <= start, finish < locations.length`
- `1 <= fuel <= 200`

Solution:

waste so much time on implemetation:

1. idx vs val get confused
    1. arr.lower_bound - arr.begin get confused, directly use *lower_bound…
    2. in helper, some time use idx, some time use val
2. thought trans to code get fucked
    1. when j==finish, i just increase by one, and not consider additional +helper(dp, locations, j, finish, newFuel), which i though it before implement, but during impl, i get lost

more solution for iterative

```cpp
using ll=long long;
ll M=1e9+7;

class Solution {
public:

    int countRoutes(vector<int>& locations, int start, int finish, int fuel) {
        int n=locations.size();//2~100
        //sort - seems not necessary
        int sv=locations[start], fv=locations[finish];
        sort(locations.begin(), locations.end());
        
        start=lower_bound(locations.begin(), locations.end(), sv)-locations.begin();
        finish=lower_bound(locations.begin(), locations.end(), fv)-locations.begin();
        if(start>finish) swap(start,finish);
        //if from to finish dist > fuel
        if(locations[finish]-locations[start]>fuel) return false;
        //process
        vector<vector<int>> dp(105,vector<int>(205,-1));
        int res=helper(dp, locations, start, finish, fuel);
        return (start==finish)?res+1:res;
    }

    ll helper(vector<vector<int>>& dp, vector<int>& locations, int i, int finish, int fuel){
        if(fuel<0) return 0;
        if(dp[i][fuel]!=-1) return dp[i][fuel];
        int n=locations.size();//2~100
        ll res=0;
        for(int j=0;j<n;j++){
            if(j==i) continue;
            int newFuel=fuel-abs(locations[j]-locations[i]);
            if(newFuel<0) continue;

            if(j==finish) res=(res+1)%M;
            res=(res+helper(dp, locations, j, finish, newFuel)) %M;
        }
        dp[i][fuel] = res;
        return dp[i][fuel];
    }
};
```