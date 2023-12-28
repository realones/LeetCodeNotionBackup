# High-Access Employees2933. High-Access Employees

#: 2933
Difficult: Medium
Tags: 2pt_SameDir_maxWin_chkValidBfrR, Hash
link: https://leetcode.com/problems/high-access-employees/description/
Priority: Low
Created time: November 12, 2023 6:59 PM
Last edited time: November 12, 2023 7:01 PM
source: LC_Contests

You are given a 2D **0-indexed** array of strings, `access_times`, with size `n`. For each `i` where `0 <= i <= n - 1`, `access_times[i][0]` represents the name of an employee, and `access_times[i][1]` represents the access time of that employee. All entries in `access_times` are within the same day.

The access time is represented as **four digits** using a **24-hour** time format, for example, `"0800"` or `"2250"`.

An employee is said to be **high-access** if he has accessed the system **three or more** times within a **one-hour period**.

Times with exactly one hour of difference are **not** considered part of the same one-hour period. For example, `"0815"` and `"0915"` are not part of the same one-hour period.

Access times at the start and end of the day are **not** counted within the same one-hour period. For example, `"0005"` and `"2350"` are not part of the same one-hour period.

Return *a list that contains the names of **high-access** employees with any order you want.*

**Example 1:**

```
Input: access_times = [["a","0549"],["b","0457"],["a","0532"],["a","0621"],["b","0540"]]
Output: ["a"]
Explanation: "a" has three access times in the one-hour period of [05:32, 06:31] which are 05:32, 05:49, and 06:21.
But "b" does not have more than two access times at all.
So the answer is ["a"].
```

**Example 2:**

```
Input: access_times = [["d","0002"],["c","0808"],["c","0829"],["e","0215"],["d","1508"],["d","1444"],["d","1410"],["c","0809"]]
Output: ["c","d"]
Explanation: "c" has three access times in the one-hour period of [08:08, 09:07] which are 08:08, 08:09, and 08:29.
"d" has also three access times in the one-hour period of [14:10, 15:09] which are 14:10, 14:44, and 15:08.
However, "e" has just one access time, so it can not be in the answer and the final answer is ["c","d"].
```

**Example 3:**

```
Input: access_times = [["cd","1025"],["ab","1025"],["cd","1046"],["cd","1055"],["ab","1124"],["ab","1120"]]
Output: ["ab","cd"]
Explanation: "ab" has three access times in the one-hour period of [10:25, 11:24] which are 10:25, 11:20, and 11:24.
"cd" has also three access times in the one-hour period of [10:25, 11:24] which are 10:25, 10:46, and 10:55.
So the answer is ["ab","cd"].
```

**Constraints:**

- `1 <= access_times.length <= 100`
- `access_times[i].length == 2`
- `1 <= access_times[i][0].length <= 10`
- `access_times[i][0]` consists only of English small letters.
- `access_times[i][1].length == 4`
- `access_times[i][1]` is in 24-hour time format.
- `access_times[i][1]` consists only of `'0'` to `'9'`.

```cpp
class Solution {
public:
    vector<string> findHighAccessEmployees(vector<vector<string>>& access_times) {
        //build mp: name-times: T:O(V) S:O(V)
        unordered_map<string, vector<int>> mp;
        for(auto& a: access_times){
            string& name=a[0], time=a[1];
            mp[name].push_back(stoi(time));
        }
        vector<string> res;
        for(auto& [name,times]: mp){
            if(isFreq(times)) res.push_back(name);
        }
        return res;
    }

    bool isFreq(vector<int>& times){
        sort(times.begin(), times.end());
        int n=times.size();
        for(int l=0, r=0; r<n; r++){
            while(!valid(times[l],times[r])) l++;
            if(r-l+1>=3) return true;
        }
        return false;
    }

    int valid(int a, int b){//b is larger, check if <60
        if(b/100 == a/100) return true;
        if(b/100 - a/100 > 1) return false;
        return (b%100 + 60 - a%100 < 60);
    }
};

/*
int n=access_times.size(); 1~100
    //access_times[i][0] name of employee : len 1~10
    //access_times[i][1] access time of employee

input is valid
================================
datadog -> ratelimiter to check if any high freq access user
================================
build mp: name-times: T:O(V) S:O(V)
for each (name, times){
    if(times has freq) add name to res
}
return res;

times has freq:
    stoi -> sort, sliding window of R, trim left, anytime slidingWin.sz>=3, true

*/
```