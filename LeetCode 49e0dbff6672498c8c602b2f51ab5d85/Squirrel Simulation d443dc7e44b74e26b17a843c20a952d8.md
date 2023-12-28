# Squirrel Simulation

#: 573
Difficult: Medium
Tags: Math, Matrix
link: https://leetcode.com/problems/squirrel-simulation/description/
Priority: Medium
Status: No Idea At First
Created time: November 12, 2023 1:25 AM
Last edited time: November 12, 2023 1:46 AM
source: leetcode_daily

You are given two integers `height` and `width` representing a garden of size `height x width`. You are also given:

- an array `tree` where `tree = [treer, treec]` is the position of the tree in the garden,
- an array `squirrel` where `squirrel = [squirrelr, squirrelc]` is the position of the squirrel in the garden,
- and an array `nuts` where `nuts[i] = [nutir, nutic]` is the position of the `ith` nut in the garden.

The squirrel can only take at most one nut at one time and can move in four directions: up, down, left, and right, to the adjacent cell.

Return *the **minimal distance** for the squirrel to collect all the nuts and put them under the tree one by one*.

The **distance** is the number of moves.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/04/24/squirrel1-grid.jpg](https://assets.leetcode.com/uploads/2021/04/24/squirrel1-grid.jpg)

```
Input: height = 5, width = 7, tree = [2,2], squirrel = [4,4], nuts = [[3,0], [2,5]]
Output: 12
Explanation: The squirrel should go to the nut at [2, 5] first to achieve a minimal distance.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/04/24/squirrel2-grid.jpg](https://assets.leetcode.com/uploads/2021/04/24/squirrel2-grid.jpg)

```
Input: height = 1, width = 3, tree = [0,1], squirrel = [0,0], nuts = [[0,2]]
Output: 3

```

**Constraints:**

- `1 <= height, width <= 100`
- `tree.length == 2`
- `squirrel.length == 2`
- `1 <= nuts.length <= 5000`
- `nuts[i].length == 2`
- `0 <= treer, squirrelr, nutir <= height`
- `0 <= treec, squirrelc, nutic <= width`

```cpp
class Solution {
public:
    int minDistance(int height, int width, vector<int>& tree, vector<int>& squirrel, vector<vector<int>>& nuts) {
        //for other nuts, go to nuts and move back
        int total=0;
        int d=INT_MAX;
        for(auto& nut: nuts){
            total+= getDist(nut,tree) *2;
            d=min(d, getDist(nut,squirrel) - getDist(nut,tree));
        }
        return total+d;
    }

    int getDist(vector<int>& start, vector<int>& end){
        return abs(start[0]-end[0]) + abs(start[1]-end[1]);
    }
};
```