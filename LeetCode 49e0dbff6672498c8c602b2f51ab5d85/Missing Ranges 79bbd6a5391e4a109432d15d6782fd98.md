# Missing Ranges

#: 163
Difficult: Easy
Tags: Interval
link: https://leetcode.com/problems/missing-ranges/
Priority: Pass
Created time: October 16, 2022 5:38 PM
Last edited time: October 12, 2023 2:56 PM
source: googleFreq, leetcode_daily

You are given an inclusive range `[lower, upper]` and a **sorted unique** integer array `nums`, where all elements are in the inclusive range.

A number `x` is considered **missing** if `x` is in the range `[lower, upper]` and `x` is not in `nums`.

Return *the **smallest sorted** list of ranges that **cover every missing number exactly***. That is, no element of `nums` is in any of the ranges, and each missing number is in one of the ranges.

Each range `[a,b]` in the list should be output as:

- `"a->b"` if `a != b`
- `"a"` if `a == b`

**Example 1:**

```
Input: nums = [0,1,3,50,75], lower = 0, upper = 99
Output: ["2","4->49","51->74","76->99"]
Explanation: The ranges are:
[2,2] --> "2"
[4,49] --> "4->49"
[51,74] --> "51->74"
[76,99] --> "76->99"

```

**Example 2:**

```
Input: nums = [-1], lower = -1, upper = -1
Output: []
Explanation: There are no missing ranges since there are no missing numbers.

```

**Constraints:**

- `109 <= lower <= upper <= 109`
- `0 <= nums.length <= 100`
- `lower <= nums[i] <= upper`
- All the values of `nums` are **unique**.

Solution: new

```cpp
class Solution {
public:
    vector<vector<int>> findMissingRanges(vector<int>& nums, int lower, int upper) {
        vector<vector<int>> res;
        nums.insert(nums.begin(), lower-1);
        nums.push_back(upper+1);
        for(int i=1; i<nums.size(); i++){
            if(nums[i-1]+1!=nums[i]){
                res.push_back({nums[i-1]+1, nums[i]-1});
            }
        }
        return res;
    }
}
```

Solution old:

```cpp
class Solution {
public:
    vector<string> findMissingRanges(vector<int>& nums, int lower, int upper) {
        int n=nums.size(); if(n==0) {return {translate(lower,upper)};}
        vector<string> res;
        //begin
        int first=nums.front(); 
        int last=nums.back();
        if(lower<first){
            res.push_back(translate(lower,first-1));
        }
        //mid
        for(int i=1;i<n;i++){
            if(nums[i-1]+1==nums[i]) continue;
            res.push_back(translate(nums[i-1]+1, nums[i]-1));
        }
        //end
        if(last<upper){res.push_back(translate(last+1,upper));}
        return res;
    }
    
    string translate(int start, int end){
        if(start==end){return to_string(start);}
        else{
            return to_string(start)+"->"+to_string(end);
        }
    }
};
```