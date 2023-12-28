# Sliding Window Median

#: 480
Difficult: Hard
Tags: 2pt_SameDir_sameSpeed, BS_todo, OrderSet
link: https://leetcode.com/problems/sliding-window-median/
Priority: High
Status: More Solution
Created time: September 1, 2022 10:24 PM
Last edited time: October 28, 2023 1:40 PM
practicedTimes: 1
source: HuaHua, jiuzhang, 算高Heap&Stack

The **median** is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle values.

- For examples, if `arr = [2,3,4]`, the median is `3`.
- For examples, if `arr = [1,2,3,4]`, the median is `(2 + 3) / 2 = 2.5`.

You are given an integer array `nums` and an integer `k`. There is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return *the median array for each window in the original array*. Answers within `10-5` of the actual value will be accepted.

**Example 1:**

```
Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [1.00000,-1.00000,-1.00000,3.00000,5.00000,6.00000]
Explanation:
Window position                Median
---------------                -----
[1  3  -1] -3  5  3  6  7        1
 1 [3  -1  -3] 5  3  6  7       -1
 1  3 [-1  -3  5] 3  6  7       -1
 1  3  -1 [-3  5  3] 6  7        3
 1  3  -1  -3 [5  3  6] 7        5
 1  3  -1  -3  5 [3  6  7]       6

```

**Example 2:**

```
Input: nums = [1,2,3,4,2,3,1,4,2], k = 3
Output: [2.00000,3.00000,3.00000,3.00000,2.00000,3.00000,2.00000]

```

**Constraints:**

- `1 <= k <= nums.length <= 105`
- `231 <= nums[i] <= 231 - 1`

Solution: 

```cpp
class Solution {
public:
    vector<double> medianSlidingWindow(vector<int>& nums, int k) {
        vector<double> res;
        if (nums.size() < k) {
            return res;
        }

        multiset<int, greater<int>> maxheap;
        multiset<int> minheap;

        for (int i = 0; i < nums.size(); i++) {
            //delete
            if (i > k-1) {
                if (maxheap.count(nums[i-k])) {
                    maxheap.erase(maxheap.find(nums[i-k]));
                } else {
                    minheap.erase(minheap.find(nums[i-k]));
                }
            }

            //add
            maxheap.insert(nums[i]);
            //rebalance
            if (maxheap.size() > minheap.size() + 1) {
                minheap.insert(*maxheap.begin());
                maxheap.erase(maxheap.begin());
            //switch top
            } else if (!maxheap.empty() && !minheap.empty() && *maxheap.begin() > *minheap.begin()) {
                minheap.insert(*maxheap.begin());
                maxheap.insert(*minheap.begin());
                maxheap.erase(maxheap.begin());
                minheap.erase(minheap.begin());
            }

            if (i >= k - 1) {
                res.push_back(getMedian(k, maxheap, minheap));
            }
        }

        return res;
    }

private:
    double getMedian(int k, multiset<int, greater<int>>& maxheap, multiset<int>& minheap) {
        if (k % 2 == 1) {
            return *maxheap.begin();
        } else {
            return ((double)*maxheap.begin() + *minheap.begin()) / 2.0;
        }
    }
};
```

Solution from discussion：

```cpp
class Solution {
public:
    vector<double> medianSlidingWindow(vector<int>& nums, int k) {
        multiset<int> window(nums.begin(), nums.begin() + k);
        auto mid = next(window.begin(), k / 2);
        vector<double> medians;
        for (int i=k; ; i++) {

            // Push the current median.
            medians.push_back((double(*mid) + *prev(mid, 1 - k%2)) / 2);

            // If all done, return.
            if (i == nums.size())
                return medians;

            // Insert nums[i].
            window.insert(nums[i]);
            if (nums[i] < *mid)
                mid--;

            // Erase nums[i-k].
            if (nums[i-k] <= *mid)
                mid++;
            window.erase(window.lower_bound(nums[i-k]));
        }
    }
};
```