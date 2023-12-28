# Frequency of the Most Frequent Element

#: 1838
Difficult: Medium
Tags: Array, BS_todo, Greedy, Prefix, Sliding Window, Sort
link: https://leetcode.com/problems/frequency-of-the-most-frequent-element/
Priority: High
Status: More Solution, No Idea At First
Created time: June 27, 2023 10:31 PM
Last edited time: November 17, 2023 10:30 PM
source: HuaHua

The **frequency** of an element is the number of times it occurs in an array.

You are given an integer array `nums` and an integer `k`. In one operation, you can choose an index of `nums` and increment the element at that index by `1`.

Return *the **maximum possible frequency** of an element after performing **at most*** `k` *operations*.

**Example 1:**

```
Input: nums = [1,2,4], k = 5
Output: 3
Explanation: Increment the first element three times and the second element two times to make nums = [4,4,4].
4 has a frequency of 3.
```

**Example 2:**

```
Input: nums = [1,4,8,13], k = 5
Output: 2
Explanation: There are multiple optimal solutions:
- Increment the first element three times to make nums = [4,4,8,13]. 4 has a frequency of 2.
- Increment the second element four times to make nums = [1,8,8,13]. 8 has a frequency of 2.
- Increment the third element five times to make nums = [1,4,13,13]. 13 has a frequency of 2.

```

**Example 3:**

```
Input: nums = [3,9,6], k = 2
Output: 1

```

**Constraints:**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 105`
- `1 <= k <= 105`

Solution sliding window new:

```cpp
using ll=long long;

class Solution {
public:
    int maxFrequency(vector<int>& nums, int k) {
        int n=nums.size();// if(n==0) {return 0;}
        sort(nums.begin(), nums.end());
        ll res=0, cost=0;
        for(int l=0,r=0;r<n;r++){
            //add r
            ll len=r-l+1;
            cost += (len-1) * (nums[r]-(r-1>=0?nums[r-1]:0));
            //trim l
            while(cost > k){
                cost -= nums[r]-nums[l]; //assume l to be trimmed
                l++;
            }
            res=max((int)res,r-l+1);
        }
        return res;
    }
};

/*
op: elem+=1
op time: <=k

return: max of elem freq
================================
nums:
    len: 1~1e5
    val: 1~1e5
k: 1~1e5
================================
sort
find a substr, that after ops they have same value, that the max len of substr
slidingWindow, max size, 
    valid = assign at most k to make them same value

add r  -> cost += (len-1)* (nums[r]-nums[r-1]);//assume r is added
while(cost > k)
    trim l -> cost -= nums[r]-nums[l]; //assume l to be trimmed
update res
cost should <= k

*/
```

Solution Sliding window:

```cpp
class Solution {
public:
    int maxFrequency(vector<int>& nums, int k) {
        int n=nums.size();// if(n==0) {return 0;}
        sort(nums.begin(), nums.end());
        int res=1;
        long long totalDiff=0;
        for(int l=0,r=0;r<n;r++){
            //update status when not dup
            if(r>0 && nums[r]>nums[r-1]){
                totalDiff += (long long)(r-l) * (nums[r]-nums[r-1]);
            }
            while(totalDiff>k){
                totalDiff -= nums[r]-nums[l];
                l++;
            }
            res=max(res,r-l+1);
        }
        return res;
    }
};

/*
n:1~1e5
val:1~1e5
k:1~1e5
================================
seq not matter -> sort/ set
3 6 9, 2
1 2 4, 5
1 4 8 13, 5

idea 1: for larger to smaller, try to inc smaller ones one by one: O(n^2)

O(n): sliding window, maintain the total diff to the largest one, it is a max window, if totalDiff<=k, update res, if tD>k, update l till valid

O(nlogn): dp_val, res from 1 to n, but not sure which one to start, skip
*/
```