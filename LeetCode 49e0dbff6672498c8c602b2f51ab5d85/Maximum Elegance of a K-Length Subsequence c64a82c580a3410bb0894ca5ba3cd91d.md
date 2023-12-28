# Maximum Elegance of a K-Length Subsequence

#: 2813
Difficult: Hard
Tags: Array, Greedy, Sort
link: https://leetcode.com/problems/maximum-elegance-of-a-k-length-subsequence/
Priority: High
Status: Dig More, No Idea At First, NotUnderstand
Created time: August 5, 2023 11:35 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given a **0-indexed** 2D integer array `items` of length `n` and an integer `k`.

`items[i] = [profiti, categoryi]`, where `profiti` and `categoryi` denote the profit and category of the `ith` item respectively.

Let's define the **elegance** of a **subsequence** of `items` as `total_profit + distinct_categories2`, where `total_profit` is the sum of all profits in the subsequence, and `distinct_categories` is the number of **distinct** categories from all the categories in the selected subsequence.

Your task is to find the **maximum elegance** from all subsequences of size `k` in `items`.

Return *an integer denoting the maximum elegance of a subsequence of* `items` *with size exactly* `k`.

**Note:** A subsequence of an array is a new array generated from the original array by deleting some elements (possibly none) without changing the remaining elements' relative order.

**Example 1:**

```
Input: items = [[3,2],[5,1],[10,1]], k = 2
Output: 17
Explanation:In this example, we have to select a subsequence of size 2.
We can select items[0] = [3,2] and items[2] = [10,1].
The total profit in this subsequence is 3 + 10 = 13, and the subsequence contains 2 distinct categories [2,1].
Hence, the elegance is 13 + 22 = 17, and we can show that it is the maximum achievable elegance.

```

**Example 2:**

```
Input: items = [[3,1],[3,1],[2,2],[5,3]], k = 3
Output: 19
Explanation: In this example, we have to select a subsequence of size 3.
We can select items[0] = [3,1], items[2] = [2,2], and items[3] = [5,3].
The total profit in this subsequence is 3 + 2 + 5 = 10, and the subsequence contains 3 distinct categories [1,2,3].
Hence, the elegance is 10 + 32 = 19, and we can show that it is the maximum achievable elegance.
```

**Example 3:**

```
Input: items = [[1,1],[2,1],[3,1]], k = 3
Output: 7
Explanation: In this example, we have to select a subsequence of size 3.
We should select all the items.
The total profit will be 1 + 2 + 3 = 6, and the subsequence contains 1 distinct category [1].
Hence, the maximum elegance is 6 + 12 = 7.
```

**Constraints:**

- `1 <= items.length == n <= 105`
- `items[i].length == 2`
- `items[i][0] == profiti`
- `items[i][1] == categoryi`
- `1 <= profiti <= 109`
- `1 <= categoryi <= n`
- `1 <= k <= n`

from lee215

```cpp
class Solution {
public:
    long long findMaximumElegance(vector<vector<int>>& A, int k) {
        sort(A.begin(), A.end(), greater<>());
        long long res = 0, cur = 0;
        vector<int> dup;//profit has dup category already recorded
        unordered_set<int> seen;//seen category

        for (int i = 0; i < A.size(); ++i) {
            int profit=A[i][0], cate=A[i][1];
            if (i < k) {//first k items
                if (seen.count(cate)) {//dup category
                    dup.push_back(profit);
                }
                cur += profit;
            } 
            //after k items
            // if new category
            else if (!seen.count(cate)) {
                if (dup.empty()) break;
                cur += profit - dup.back();
                dup.pop_back();
            }
            seen.insert(cate);
            res = fmax(res, cur + 1L * seen.size() * seen.size());
        }
        return res;
    }
};
```