# Jump Game II

#: 45
Difficult: Medium
Tags: Array, DP, GoForth, Greedy, buildFutureResults
link: https://leetcode.com/problems/jump-game-ii
Priority: Medium
Status: More Solution
Created time: August 17, 2022 11:31 PM
Last edited time: November 17, 2023 1:28 AM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, jiuzhang, ticktokFreq, 算初DP

Given an array of non-negative integers `nums`, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

You can assume that you can always reach the last index.

**Example 1:**

```
Input: nums = [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.

```

**Example 2:**

```
Input: nums = [2,3,0,1,4]
Output: 2

```

**Constraints:**

- `1 <= nums.length <= 104`
- `0 <= nums[i] <= 1000`

```cpp
//solution 1 greedy
class Solution {
public:
    /*
     * @param A: A list of integers
     * @return: An integer
     */
    int jump(vector<int> &A) {
        // write your code here
        int start=0,end=0,steps=0;
        while(end<A.size()-1){
            steps++;
            int farthest=0;
            for(int i=start;i<=end;i++){
                farthest=max(farthest,i+A[i]);
            }
            start=end+1;
            end=farthest;
        }
        return steps;
    }
};
```

```cpp
//solution 2
//T(O)=n^2, will exceed time limit
public class Solution {
    public int jump(int[] A) {
        // state
        int[] steps = new int[A.length];

        // initialize
        steps[0] = 0;
        for (int i = 1; i < A.length; i++) {
            steps[i] = Integer.MAX_VALUE;
        }

        // function
        for (int i = 1; i < A.length; i++) {
            for (int j = 0; j < i; j++) {
                if (steps[j] != Integer.MAX_VALUE && j + A[j] >= i) {
                    steps[i] = Math.min(steps[i], steps[j] + 1);
                }
            }
        }

        // answer
        return steps[A.length - 1];
    }
}
```