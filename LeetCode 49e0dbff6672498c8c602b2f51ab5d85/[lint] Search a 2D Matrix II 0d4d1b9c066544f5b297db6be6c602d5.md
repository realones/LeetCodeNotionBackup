# [lint] Search a 2D Matrix II

#: 38
Difficult: Medium
Tags: BS_Idx
link: https://www.lintcode.com/problem/38/
Priority: Low
Created time: June 26, 2022 11:00 AM
Last edited time: October 12, 2023 2:57 PM
practicedTimes: 1
source: jiuzhang, 算初BS

**Description**

Write an efficient algorithm that searches for a value in an m x n matrix, return The number of occurrence of it.

This matrix has the following properties:

- Integers in each row are sorted from left to right.
- Integers in each column are sorted from up to bottom.
- No duplicate integers in each row or column.

---

- 

*Contact me on wechat to get **Amazon、Google** requent Interview questions . (wechat id : **jiuzhang0607**)*

**Example**

**Example 1:**

Input:

```
matrix = [[3,4]]
target = 3

```

Output:

```
1

```

Explanation:

There is only one 3 in the matrix.

**Example 2:**

Input:

```
matrix = [
      [1, 3, 5, 7],
      [2, 4, 7, 8],
      [3, 5, 9, 10]
    ]
target = 3

```

Output:

```
2

```

Explanation:

There are two 3 in the matrix.

**Challenge**

O(m+n) time and O(1) extra space

```cpp
public class Solution {
    /*
     * @param matrix: A list of lists of integers
     * @param target: An integer you want to search in matrix
     * @return: An integer indicate the total occurrence of target in the given matrix
     */
    public int searchMatrix(int[][] matrix, int target) {
        int row = matrix.length-1;
        int col = 0;
        int cnt=0;
        while (row >= 0 && col < matrix[0].length) {
            
            if (matrix[row][col] > target) {
                row--;
            } else if (matrix[row][col] < target) {
                col++;
            } else { // found it
                cnt++;
                row--;
                col++;
            }
        }

        return cnt;
    }
}
```