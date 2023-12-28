# Max Consecutive Ones II

#: 487
Difficult: Medium
Tags: 2pt, DP, dp-TimeSeqFromLastOne-Power, stateMachine
link: https://leetcode.com/problems/max-consecutive-ones-ii/
Priority: Low
Created time: November 19, 2022 11:05 PM
Last edited time: November 28, 2023 8:27 PM
source: wp动归套路

Given a binary array `nums`, return *the maximum number of consecutive* `1`*'s in the array if you can flip at most one* `0`.

**Example 1:**

```
Input: nums = [1,0,1,1,0]
Output: 4
Explanation:
- If we flip the first zero, nums becomes [1,1,1,1,0] and we have 4 consecutive ones.
- If we flip the second zero, nums becomes [1,0,1,1,1] and we have 3 consecutive ones.
The max number of consecutive ones is 4.

```

**Example 2:**

```
Input: nums = [1,0,1,1,0,1]
Output: 4
Explanation:
- If we flip the first zero, nums becomes [1,1,1,1,0,1] and we have 4 consecutive ones.
- If we flip the second zero, nums becomes [1,0,1,1,1,1] and we have 4 consecutive ones.
The max number of consecutive ones is 4.

```

**Constraints:**

- `1 <= nums.length <= 105`
- `nums[i]` is either `0` or `1`.

**Follow up:** What if the input numbers come in one by one as an infinite stream? In other words, you can't store all numbers coming from the stream as it's too large to hold in memory. Could you solve it efficiently?

Solution: statemachine

```cpp
/*
state:(max continueous point including cur)
    not_flipped (init)
    flipped

action:
    use cur num
        n->n, f->f
    use the flipped cur num
        n->f
*/
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int flipped=INT_MIN, notFlipped=0, res=0; //flipped=0 also works
        
        for(auto num: nums){
            if(num==0){
                flipped=notFlipped+1;
                notFlipped=0;
            }
            else{//1
                flipped+=1;
                notFlipped+=1;
            }
            res=max({res, flipped, notFlipped});
        }
        
        return res;
    }
};
```

Solution sliding win:

```cpp
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int n=nums.size();// if(n==0) {return 0;}
        int cnt0=0;
        int res=0;
        for(int l=0,r=0; r<n; r++){
            //add nums[r]
            cnt0 += (nums[r]==0);
            //while not qualifi, trim l
            while(cnt0==2){
                if(nums[l]==0) cnt0--;
                l++;
            }
            //update res
            res=max(res,r-l+1);
        }
        return res;
    }
};
```