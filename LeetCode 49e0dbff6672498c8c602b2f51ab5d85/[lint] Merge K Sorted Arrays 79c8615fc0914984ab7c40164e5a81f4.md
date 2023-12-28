# [lint] Merge K Sorted Arrays

#: 486
Difficult: Medium
Tags: MergeSort, PQ
link: https://www.lintcode.com/problem/merge-k-sorted-arrays
Priority: Low
Created time: August 15, 2022 1:10 PM
Last edited time: October 20, 2023 7:34 PM
practicedTimes: 1
source: jiuzhang, 算初Hash&Heap

Given *k* sorted integer arrays, merge them into one sorted array.

Example:
**Example 1:**

```
Input:
 [
   [1, 3, 5, 7],
   [2, 4, 6],
   [0, 8, 9, 10, 11]
 ]
Output: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]

```

**Example 2:**

```
Input:
 [
   [1,2,3],
   [1,2]
 ]
Output: [1,1,2,2,3]

```

Challenge
Do it in O(N log k).

- *N* is the total number of integers.
- *k* is the number of arrays.

```cpp
class Node {
public:
    int row, col, val;
    Node (int r, int c, int v) : row(r), col(c), val(v) {};
    bool operator < (const Node &obj) const {
        return val > obj.val;
    }
};

class Solution {
public:
    /**
     * @param arrays k sorted integer arrays
     * @return a sorted array
     */
    vector<int> mergekSortedArrays(vector<vector<int>>& arrays) {
        vector<int> result;
        if (arrays.empty())
            return result;

        priority_queue<Node> queue;
        for (int i = 0; i < arrays.size(); ++i) {
            if (!arrays[i].empty())
                queue.push(Node(i, 0, arrays[i][0]));
        }

        while (!queue.empty()) {
            Node curr = queue.top();
            queue.pop();
            result.push_back(curr.val);
            if (curr.col + 1 < arrays[curr.row].size())
                queue.push(Node(curr.row, curr.col + 1,
                            arrays[curr.row][curr.col + 1]));
        }

        return result;
    }
};
```