# Maximum Sum Queries

#: 2736
Difficult: Hard
link: https://leetcode.com/problems/maximum-sum-queries/
Status: new
Created time: June 10, 2023 9:32 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given two **0-indexed** integer arrays `nums1` and `nums2`, each of length `n`, and a **1-indexed 2D array** `queries` where `queries[i] = [xi, yi]`.

For the `ith` query, find the **maximum value** of `nums1[j] + nums2[j]` among all indices `j` `(0 <= j < n)`, where `nums1[j] >= xi` and `nums2[j] >= yi`, or **-1** if there is no `j` satisfying the constraints.

Return *an array* `answer` *where* `answer[i]` *is the answer to the* `ith` *query.*

**Example 1:**

```
Input: nums1 = [4,3,1,2], nums2 = [2,4,9,5], queries = [[4,1],[1,3],[2,5]]
Output: [6,10,7]
Explanation:
For the 1st queryxi = 4 andyi = 1, we can select indexj = 0 sincenums1[j] >= 4 andnums2[j] >= 1. The sumnums1[j] + nums2[j] is 6, and we can show that 6 is the maximum we can obtain.

For the 2nd queryxi = 1 andyi = 3, we can select indexj = 2 sincenums1[j] >= 1 andnums2[j] >= 3. The sumnums1[j] + nums2[j] is 10, and we can show that 10 is the maximum we can obtain.

For the 3rd queryxi = 2 andyi = 5, we can select indexj = 3 sincenums1[j] >= 2 andnums2[j] >= 5. The sumnums1[j] + nums2[j] is 7, and we can show that 7 is the maximum we can obtain.

Therefore, we return[6,10,7].

```

**Example 2:**

```
Input: nums1 = [3,2,5], nums2 = [2,3,4], queries = [[4,4],[3,2],[1,1]]
Output: [9,9,9]
Explanation: For this example, we can use indexj = 2 for all the queries since it satisfies the constraints for each query.

```

**Example 3:**

```
Input: nums1 = [2,1], nums2 = [2,3], queries = [[3,3]]
Output: [-1]
Explanation: There is one query in this example withxi = 3 andyi = 3. For every index, j, either nums1[j] <xi or nums2[j] <yi. Hence, there is no solution.

```

**Constraints:**

- `nums1.length == nums2.length`
- `n == nums1.length`
- `1 <= n <= 105`
- `1 <= nums1[i], nums2[i] <= 109`
- `1 <= queries.length <= 105`
- `queries[i].length == 2`
- `xi == queries[i][1]`
- `yi == queries[i][2]`
- `1 <= xi, yi <= 109`

```cpp

```