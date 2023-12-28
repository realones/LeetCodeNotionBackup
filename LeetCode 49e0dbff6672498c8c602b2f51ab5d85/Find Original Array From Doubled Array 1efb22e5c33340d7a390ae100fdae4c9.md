# Find Original Array From Doubled Array

#: 2007
Difficult: Medium
Tags: Array, Greedy, Hash, OrderMap, OrderSet, Sort
link: https://leetcode.com/problems/find-original-array-from-doubled-array/description/
Priority: Medium
Status: More Solution
Created time: December 15, 2023 11:28 PM
Last edited time: December 16, 2023 12:01 AM
source: microsoftFreq

An integer array `original` is transformed into a **doubled** array `changed` by appending **twice the value** of every element in `original`, and then randomly **shuffling** the resulting array.

Given an array `changed`, return `original` *if* `changed` *is a **doubled** array. If* `changed` *is not a **doubled** array, return an empty array. The elements in* `original` *may be returned in **any** order*.

**Example 1:**

```
Input: changed = [1,3,4,2,6,8]
Output: [1,3,4]
Explanation: One possible original array could be [1,3,4]:
- Twice the value of 1 is 1 * 2 = 2.
- Twice the value of 3 is 3 * 2 = 6.
- Twice the value of 4 is 4 * 2 = 8.
Other original arrays could be [4,3,1] or [3,1,4].

```

**Example 2:**

```
Input: changed = [6,3,0,1]
Output: []
Explanation: changed is not a doubled array.

```

**Example 3:**

```
Input: changed = [1]
Output: []
Explanation: changed is not a doubled array.

```

**Constraints:**

- `1 <= changed.length <= 105`
- `0 <= changed[i] <= 105`

Solution OrderMap:

```cpp
class Solution {
public:
    vector<int> findOriginalArray(vector<int>& nums) {
        int n=nums.size();// if(n==0) {return 0;}
        if(n%2==1) return {};
        vector<int> res;
        map<int,int> mp;
        for(auto x: nums){ mp[x]++; }
        //handle 0
        if(mp.count(0)){
            if(mp[0]%2==1) return {};
            for(int t=0;t<mp[0]/2;t++){
                res.push_back(0);
            }
        }
        mp.erase(0);
        //handle others
        while(mp.size()>0){
            auto [x,cnt]=*mp.begin();
            mp.erase(mp.begin());
            if(mp.count(x*2) && mp[x*2]>=cnt){
                mp[x*2]-=cnt;
                if(mp[x*2]==0) mp.erase(x*2);
            }
            else return {};
            for(int t=0;t<cnt;t++){
                res.push_back(x);
            }
        }
        return res;
    }
};

/*
set: seq, erase
    getSmallest - guarantee it belong to origin - O(1)
    erase it and it's double  O(logn)
iterative call n/2 times - O(n)
===> T:O(nlogn) S:O(n)
*/
```

solution TLE: OrderSet

```cpp
class Solution {
public:
    vector<int> findOriginalArray(vector<int>& nums) {
        int n=nums.size();// if(n==0) {return 0;}
        if(n%2==1) return {};
        multiset<int> st(nums.begin(), nums.end());
        vector<int> res;
        for(int t=0;t<n/2;t++){
            int x=*st.begin();
            st.erase(st.begin());
            if(st.count(x*2)){
                st.erase(st.find(x*2));
            }
            else return {};
            res.push_back(x);
        }
        return res;
    }
};

/*
set: seq, erase
    getSmallest - guarantee it belong to origin - O(1)
    erase it and it's double  O(logn)
iterative call n/2 times - O(n)
===> T:O(nlogn) S:O(n)
*/
```