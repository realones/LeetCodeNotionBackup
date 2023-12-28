# Maximum Length of a Concatenated String with Unique Characters

#: 1239
Difficult: Medium
Tags: Array, BackTracking, Bit Manipulation, search, string
link: https://leetcode.com/problems/maximum-length-of-a-concatenated-string-with-unique-characters/description/
Priority: Medium
Status: HardToImplement
Created time: June 27, 2023 10:49 PM
Last edited time: December 13, 2023 11:08 PM
source: HuaHua, microsoftFreq

You are given an array of strings `arr`. A string `s` is formed by the **concatenation** of a **subsequence** of `arr` that has **unique characters**.

Return *the **maximum** possible length* of `s`.

A **subsequence** is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

```
Input: arr = ["un","iq","ue"]
Output: 4
Explanation: All the valid concatenations are:
- ""
- "un"
- "iq"
- "ue"
- "uniq" ("un" + "iq")
- "ique" ("iq" + "ue")
Maximum length is 4.

```

**Example 2:**

```
Input: arr = ["cha","r","act","ers"]
Output: 6
Explanation: Possible longest valid concatenations are "chaers" ("cha" + "ers") and "acters" ("act" + "ers").

```

**Example 3:**

```
Input: arr = ["abcdefghijklmnopqrstuvwxyz"]
Output: 26
Explanation: The only string in arr has all 26 characters.

```

**Constraints:**

- `1 <= arr.length <= 16`
- `1 <= arr[i].length <= 26`
- `arr[i]` contains only lowercase English letters.

```cpp
class Solution {
public:
    int maxLength(vector<string>& arr) {
        //remove invalid inputs
        unordered_set<string> st;
        for(auto& s: arr){ if(valid(s)) st.insert(s); }
        vector<int> nums;
        for(auto s: st){ nums.push_back(trans(s)); }
        //backtracking
        int res=0;
        int mask=0;
        helper(nums, res, mask, 0);
        return res;
    }

    void helper(vector<int>& nums, int& res, int& mask, int start){
        res=max(res, getLen(mask));
        for(int i=start;i<nums.size();i++){
            if((nums[i] & mask) == 0){
                mask+=nums[i];
                helper(nums, res, mask, i+1);
                mask-=nums[i];
            }
        }
    }

    bool valid(string& s){//todo use bit mask
        unordered_set<char> st;
        for(auto c: s){
            if(st.count(c)) return false;
            st.insert(c);
        }
        return true;
    }

    int trans(string& s){
        int x=0;
        for(auto c: s){
            x+= 1<<(c-'a');
        }
        return x;
    }

    int getLen(int mask){
        int cnt=0;
        while(mask){
            cnt+=(mask&1);
            mask>>=1;
        }
        return cnt;
    }
};

/*
backtracking + bit
*/
```