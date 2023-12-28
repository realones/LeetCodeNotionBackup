# Number of Flowers in Full Bloom

#: 2251
Difficult: Hard
Tags: Array, BS_lower_upper_bound, Hash, OrderSet, Prefix, Sort
link: https://leetcode.com/problems/number-of-flowers-in-full-bloom/description/
Priority: Medium
Status: More Solution
Created time: December 16, 2023 12:17 AM
Last edited time: December 16, 2023 12:51 AM
source: microsoftFreq

You are given a **0-indexed** 2D integer array `flowers`, where `flowers[i] = [starti, endi]` means the `ith` flower will be in **full bloom** from `starti` to `endi` (**inclusive**). You are also given a **0-indexed** integer array `people` of size `n`, where `people[i]` is the time that the `ith` person will arrive to see the flowers.

Return *an integer array* `answer` *of size* `n`*, where* `answer[i]` *is the **number** of flowers that are in full bloom when the* `ith` *person arrives.*

**Example 1:**

![https://assets.leetcode.com/uploads/2022/03/02/ex1new.jpg](https://assets.leetcode.com/uploads/2022/03/02/ex1new.jpg)

```
Input: flowers = [[1,6],[3,7],[9,12],[4,13]], people = [2,3,7,11]
Output: [1,2,2,2]
Explanation:The figure above shows the times when the flowers are in full bloom and when the people arrive.
For each person, we return the number of flowers in full bloom during their arrival.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2022/03/02/ex2new.jpg](https://assets.leetcode.com/uploads/2022/03/02/ex2new.jpg)

```
Input: flowers = [[1,10],[3,3]], people = [3,3,2]
Output: [2,2,1]
Explanation: The figure above shows the times when the flowers are in full bloom and when the people arrive.
For each person, we return the number of flowers in full bloom during their arrival.

```

**Constraints:**

- `1 <= flowers.length <= 5 * 104`
- `flowers[i].length == 2`
- `1 <= starti <= endi <= 109`
- `1 <= people.length <= 5 * 104`
- `1 <= people[i] <= 109`

```cpp
class Solution {
public:
    vector<int> fullBloomFlowers(vector<vector<int>>& flowers, vector<int>& people) {
        int n=flowers.size();
        vector<int> starts, ends;
        for(auto& f: flowers){
            starts.push_back(f[0]);//to check starting point with p
            ends.push_back(f[1]);//to check ending point with p
        }

        sort(starts.begin(), starts.end());
        sort(ends.begin(), ends.end());

        vector<int> res;
        for(auto p: people){
            //right
            auto iter1=upper_bound(starts.begin(), starts.end(),p);//right is the first one with starting point > p
            int cnt1=starts.end()-iter1;
            //left
            auto iter2=lower_bound(ends.begin(), ends.end(),p);//left is the last one with ending point < p
            int cnt2=iter2-ends.begin();
            //add to res
            res.push_back(n-cnt1-cnt2);
        }
        return res;
    }
};

/*
flowers[i]: begin~end
people[i]: time
================================
flowers:
    len: 1e5
    val: 1e9
people:
    len: 1e5
    val: 1e9
================================
solutions 1. 
    mark all time with cnt of flowers
        T:O(flower.cnt* flower.aveLen)
        S:O(1e9)
    for each people, check num of flowers O(1)
    T:O(flower.cnt* flower.aveLen)
    S:O(1e9)
solutions 2.
    for each people O(1e5)
        for each flowers, if in range O(1e5)
    T:O(people.cnt*flower.cnt)
    S:O(1)
solutions 3:
    sort flowers nlogn, n is flower.len
        sort by start, then by end
    for each people O(people.len)
        bs right out of range one O(log(flower.len))
        bs left out of range one O(log(flower.len))
    T: O(people.len * log(flower.len))
    S: O(1)
*/
```