# Find the Duplicate Number

#: 287
Difficult: Medium
Tags: BS_Val, Cycle
link: https://leetcode.com/problems/find-the-duplicate-number/
Priority: Low
Status: More Solution
Created time: September 7, 2022 9:44 PM
Last edited time: October 28, 2023 5:21 PM
practicedTimes: 1
source: jiuzhang, 算高BS&SwipeLine

Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

There is only **one repeated number** in `nums`, return *this repeated number*.

You must solve the problem **without** modifying the array `nums` and uses only constant extra space.

**Example 1:**

```
Input: nums = [1,3,4,2,2]
Output: 2

```

**Example 2:**

```
Input: nums = [3,1,3,4,2]
Output: 3

```

**Constraints:**

- `1 <= n <= 105`
- `nums.length == n + 1`
- `1 <= nums[i] <= n`
- All the integers in `nums` appear only **once** except for **precisely one integer** which appears **two or more** times.

**Follow up:**

- How can we prove that at least one duplicate number must exist in `nums`?
- Can you solve the problem in linear runtime complexity?

1. hashset 和 sort 是实际生活中解决的时候很容易想到的方法。
2. 一般val和index有一一对应关系的，基本上会用到指针来跳转。至于快慢双指针找环出现的node，比较难想，需要公式推导：
    1. 如果不多那一个数，那么研究一下，可以发现每个node的indegree和outdegree都为1.
    2. 也就是说，一定会形成一个或多个大大小小的环，都是简单环，就是只有一条线画圆。
    3. 注意 val和index的对应关系是个map，类似于node的指针，本质上是这些val作为node形成环。
    4. 然后这时候多加了一个数进去，可以想像，如果
        1. val从1开始算，那么index=0的地方，indegree=0，outdegree=1
        2. val从0开始算，那么最后一个index=n的地方，indegree=0，outdegree=1
        3. 上面两个是等价的
    5. 也就是说，它（假设是nodeK）加入了图中，因为它贡献了一个outdegree，已有的他练到的图中肯定有一个node是indegree是2，也就是出现环的地方
    6. 可以想几个例子：
        1. 环最大的情况，nodeK直接指向环的开始位置
        2. 最小的情况，
        3. 和一般的情况。
    7. 思路上，不好直接想，因为可能有多个不相关的环出现，主要是通过indegree和outdegree来进行抽象推导。
3. 至于BS的方法，也是NlogN，但是本身复杂度很高，只有在题目约束S(O)=1, 和input readonly的时候开始想这个做法。
    1. 也是想到暴力一个个数字找会有重复计算，所以二分：给定K，找小于它的数的个数

```cpp
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int n=nums.size();
        int slow=0, fast=nums[slow];
        while(slow != fast){
            //check end not required
            slow=nums[slow];
            fast=nums[nums[fast]];
        }
        slow=0, fast=nums[fast];
        while(slow!=fast){
            slow=nums[slow];
            fast=nums[fast];
        }
        return slow;
    }
};
```

```cpp
//two solutions

//solution 1 - two points, fast and slow
class Solution {
public:
    int findDuplicate(vector<int>& nums){
        if (nums.size() > 1)	{
    		int slow = nums[0];//from 0+1
    		int fast = nums[nums[0]];//from 0+1*2
    		while (slow != fast)
    		{
    			slow = nums[slow];
    			fast = nums[nums[fast]];
    		}
    
    		fast = 0;
    		while (fast != slow)
    		{
    			fast = nums[fast];
    			slow = nums[slow];
    		}
    		return slow;
    	}
    	return -1;
    }
};

// 二分法
// class Solution2 {
//     /**
//      * @param nums an array containing n + 1 integers which is between 1 and n
//      * @return the duplicate one
//      */
//     public int findDuplicate2(int[] nums) {
//         // Write your code here
//         int start = 1;
//         int end = nums.length - 1;
//         while(start + 1 < end) {
//             int mid = start + (end - start) / 2;
//             if (check_smaller_num(mid, nums) <= mid) {
//                 start = mid;
//             } else {
//                 end = mid;
//             }
//         }
            
//         if (check_smaller_num(start, nums) <= start) {
//             return end;
//         }
//         return start;
//     }
    
//     public int check_smaller_num(int mid, int[] nums) {
//         int cnt = 0;
//         for(int i = 0; i < nums.length; i++){
//             if(nums[i] <= mid){
//                 cnt++;
//             }
//         }
//         return cnt;
//     }
// };
```