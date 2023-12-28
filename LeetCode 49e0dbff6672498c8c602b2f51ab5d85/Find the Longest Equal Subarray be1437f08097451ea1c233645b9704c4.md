# Find the Longest Equal Subarray

#: 2831
Difficult: Medium
Tags: 2pt_SameDir_maxWin_chkValidAftR, Hash, Sliding Window, monoWindow
link: https://leetcode.com/problems/find-the-longest-equal-subarray/
Priority: High
Status: Dig More, No Idea At First
Created time: August 19, 2023 9:42 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given a **0-indexed** integer array `nums` and an integer `k`.

A subarray is called **equal** if all of its elements are equal. Note that the empty subarray is an **equal** subarray.

Return *the length of the **longest** possible equal subarray after deleting **at most*** `k` *elements from* `nums`.

A **subarray** is a contiguous, possibly empty sequence of elements within an array.

**Example 1:**

```
Input: nums = [1,3,2,3,1,3], k = 3
Output: 3
Explanation: It's optimal to delete the elements at index 2 and index 4.
After deleting them, nums becomes equal to [1, 3, 3, 3].
The longest equal subarray starts at i = 1 and ends at j = 3 with length equal to 3.
It can be proven that no longer equal subarrays can be created.

```

**Example 2:**

```
Input: nums = [1,1,2,2,1,1], k = 2
Output: 4
Explanation: It's optimal to delete the elements at index 2 and index 3.
After deleting them, nums becomes equal to [1, 1, 1, 1].
The array itself is an equal subarray, so the answer is 4.
It can be proven that no longer equal subarrays can be created.

```

**Constraints:**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= nums.length`
- `0 <= k <= nums.length`

**Solution**:

Solution multiple sliding windows:

from wisdompeak:

Intuition:

- for value A, find max window that start from A and end with A, make it has as many A as possible, meanwhile other elements cnt <= k.

Approach:

- For each value in the nums, maintain a array for the indexes of that value.
Then for index array of each value, we find the maximum window that the idx range - idx cnt <= k.

Complexity
Time complexity:
- put idx into hash map => O(n)
- max sliding window processing, in total n elements => O(n)

Space complexity:
- O(n)

```cpp
class Solution {
public:
    int longestEqualSubarray(vector<int>& nums, int k) {
        unordered_map<int,vector<int>> mp;
        for(int i=0; i<nums.size(); i++){
            mp[nums[i]].push_back(i);
        }

        int res=0;
        for(auto [_,idxs]: mp){
            for(int l=0,r=0,cnt=0;r<idxs.size();r++){
                //add r
                //while invalid trim l
                while((idxs[r]-idxs[l]+1) - (r-l+1) > k){
                    l++;
                }
                //update res
                res=max(res,r-l+1);
            }
        }
        return res;
    }
};

/*
X [A X X A X A] X X X A X X
       i         j

idx:
A: 1 4 6 10
   i j
   find max window with idx_Range - A_cnt <= k
B ...
...
*/
```

Solution lee215:

mono inc sliding window

key: maintain maxF, 

trick: 

Note that the size of window won't decrease,
and maxf is actually the max frequency in history (not the current subarray),

```cpp
class Solution {
public:
    int longestEqualSubarray(vector<int>& nums, int k) {
       int n=nums.size();
       int maxf=0;
       unordered_map<int,int> freqs;
       for(int l=0,r=0;r<n;r++){
           //add right
           freqs[nums[r]]++;
           maxf=max(maxf,freqs[nums[r]]);
           //if windowSz-maxf>k, trim l
           if(r-l+1-maxf>k){
               freqs[nums[l]]--;
               l++;
           }
           //update res,
       }
       return maxf;
    }
};
```

Solution TLE:

```cpp
class Solution {
public:
    int longestEqualSubarray(vector<int>& nums, int k) {
       int n=nums.size();
       int maxf=0,res=0;
       unordered_map<int,int> freqs;
       for(int l=0,r=0;r<n;r++){
           //add right
           freqs[nums[r]]++;
           maxf=max(maxf,freqs[nums[r]]);
           //if windowSz-maxf>k, trim l
           if(r-l+1-maxf>k){
               freqs[nums[l]]--;
               l++;
           }
           maxf=-1;
           for(auto [_,freq]: freqs){
               maxf=max(maxf,freq);
           }
           //update res,
           res=max(res,maxf);
       }
       return res;
    }
};
```

Similar to the question and solutions for

- [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/discuss/278271/JavaC%2B%2BPython-Sliding-Window-just-O(n)),
- [2024. Maximize the Confusion of an Exam](https://leetcode.com/problems/maximize-the-confusion-of-an-exam/discuss/1499049/JavaC%2B%2BPython-Sliding-Window-strict-O(n))

More Similar Sliding Window Problems

- [Count Complete Subarrays in an Array](https://leetcode.com/problems/count-complete-subarrays-in-an-array/discuss/3836137/JavaC%2B%2BPython-Sliding-Window-O(n))
- [Maximum Beauty of an Array After Applying Operation](https://leetcode.com/problems/maximum-beauty-of-an-array-after-applying-operation/discuss/3771308/JavaC%2B%2BPython-Sliding-Window)
- [Find the Longest Semi-Repetitive Substring](https://leetcode.com/problems/find-the-longest-semi-repetitive-substring/discuss/3629621/JavaC%2B%2BPython-Sliding-Window)
- [Maximize Win From Two Segments](https://leetcode.com/problems/maximize-win-from-two-segments/discuss/3141449/JavaC%2B%2BPython-DP-%2B-Sliding-Segment-O(n))
- [Count the Number of Good Subarrays](https://leetcode.com/problems/count-the-number-of-good-subarrays/discuss/3052559/C%2B%2BPython-Sliding-Window)
- [Longest Nice Subarray](https://leetcode.com/problems/longest-nice-subarray/discuss/2527496/Python-Sliding-Window)
- [Maximum Number of Robots Within Budget](https://leetcode.com/problems/maximum-number-of-robots-within-budget/discuss/2524838/Python-Sliding-Window-O(n))
- [Frequency of the Most Frequent Element](https://leetcode.com/problems/frequency-of-the-most-frequent-element/discuss/1175090/JavaC%2B%2BPython-Sliding-Window)
- [Longest Subarray of 1's After Deleting One Element](https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/discuss/708112/JavaC%2B%2BPython-Sliding-Window-at-most-one-0)
- [Constrained Subsequence Sum](https://leetcode.com/problems/constrained-subsequence-sum/discuss/597751/JavaC++Python-O(N)-Decreasing-Deque)
- [Number of Substrings Containing All Three Characters](https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/discuss/516977/JavaC++Python-Easy-and-Concise)
- [Count Number of Nice Subarrays](https://leetcode.com/problems/count-number-of-nice-subarrays/discuss/419378/JavaC%2B%2BPython-Sliding-Window-atMost(K)-atMost(K-1))
- [Replace the Substring for Balanced String](https://leetcode.com/problems/replace-the-substring-for-balanced-string/discuss/408978/javacpython-sliding-window/)
- [Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/discuss/247564/JavaC%2B%2BPython-Sliding-Window)
- [Binary Subarrays With Sum](https://leetcode.com/problems/binary-subarrays-with-sum/discuss/186683/)
- [Subarrays with K Different Integers](https://leetcode.com/problems/subarrays-with-k-different-integers/discuss/523136/JavaC%2B%2BPython-Sliding-Window)
- [Fruit Into Baskets](https://leetcode.com/problems/fruit-into-baskets/discuss/170740/Sliding-Window-for-K-Elements)
- [Shortest Subarray with Sum at Least K](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/discuss/143726/C%2B%2BJavaPython-O(N)-Using-Deque)
-