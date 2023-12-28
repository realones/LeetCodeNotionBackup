# Maximum AND Sum of Array

#: 2172
Difficult: Hard
Tags: Bit Manipulation, dp-bitMask, two_divided_graph
link: https://leetcode.com/problems/maximum-and-sum-of-array/
Priority: High
Status: No Idea At First, toSummarize
Created time: October 9, 2022 3:08 PM
Last edited time: October 12, 2023 2:56 PM
source: googleFreq

You are given an integer array `nums` of length `n` and an integer `numSlots` such that `2 * numSlots >= n`. There are `numSlots` slots numbered from `1` to `numSlots`.

You have to place all `n` integers into the slots such that each slot contains at **most** two numbers. The **AND sum** of a given placement is the sum of the **bitwise** `AND` of every number with its respective slot number.

- For example, the **AND sum** of placing the numbers `[1, 3]` into slot  and `[4, 6]` into slot  is equal to `(1 AND 1) + (3 AND 1) + (4 AND 2) + (6 AND 2) = 1 + 1 + 0 + 2 = 4`.
    
    

Return *the maximum possible **AND sum** of* `nums` *given* `numSlots` *slots.*

**Example 1:**

```
Input: nums = [1,2,3,4,5,6], numSlots = 3
Output: 9
Explanation: One possible placement is [1, 4] into slot1, [2, 6] into slot2, and [3, 5] into slot3.
This gives the maximum AND sum of (1 AND1) + (4 AND1) + (2 AND2) + (6 AND2) + (3 AND3) + (5 AND3) = 1 + 0 + 2 + 2 + 3 + 1 = 9.

```

**Example 2:**

```
Input: nums = [1,3,10,4,7,1], numSlots = 9
Output: 24
Explanation: One possible placement is [1, 1] into slot1, [3] into slot3, [4] into slot4, [7] into slot7, and [10] into slot9.
This gives the maximum AND sum of (1 AND1) + (1 AND1) + (3 AND3) + (4 AND4) + (7 AND7) + (10 AND9) = 1 + 1 + 3 + 4 + 7 + 8 = 24.
Note that slots 2, 5, 6, and 8 are empty which is permitted.

```

**Constraints:**

- `n == nums.length`
- `1 <= numSlots <= 9`
- `1 <= n <= 2 * numSlots`
- `1 <= nums[i] <= 15`

Solution 1: DP+BitMask

```cpp
class Solution {
public:
    int maximumANDSum(vector<int>& nums, int numSlots) {
        int numSz=nums.size();
        //nums.insert(nums.begin(),0); seems work better for matrix dp
        int slotSz=numSlots;
        
        vector<vector<int>> dp(numSz+1, vector<int>(pow(3,slotSz), INT_MIN));
        
        dp[0][0]=0;
        
        for(int i=1; i<numSz+1; i++){//ith num
            int num=nums[i-1];
            for(int state=0; state<pow(3,slotSz); state++){
                for(int slot=0; slot<slotSz; slot++){//slot idx
                    if(numCntInSlot(state,slot)>=1){
                        dp[i][state] = max(dp[i][state], dp[i-1][state - pow(3,slot)] + (num&(slot+1)));//*& priority lower than +*
                    }
                }
            }
        }
        
        return *max_element(dp.back().begin(), dp.back().end());
    }
    
    int numCntInSlot(int state, int slot){
        for(int i=0; i<slot;i++){
            state/=3;
        }
        return state%3;
    }
};

/*
DP+BitMask(路径压缩)
binary division graph, *can use both side as first key*

1. iterate over slots, using state to represent nums, which nums are already in slots
                            101010011101010011
    for slot: 1~9
        for numState to represent nums already in slots: 1~2^18 - bitmask
            for pickedNum for i-th slot: available plans
                dp[slot][NumState] max= dp[slot-1][NumState - pickNum] + pickNum&slot
                
    T= 9 * 2^18 * (C(18,2)+C(18,1)+C(18,0))=7e8 -> TLE
       
       
2. iterator over nums, using state to represent slots
                            012210210
    for num: 1~18
        for slotState: 1~3^9
            for slot: nums in slot >=1
                dp[num][slotState] max= dp[num-1][slotState with slot-1] + num&slot
    
    T= 18 * 3^9 * 9 = 3e6 -> workable solution
                
                
BitMask(路径压缩)
    dp[i][state]
    for i
        for state
            dp[i][state]=dp[i-1][???]
*/
```

Solution 2: improvement

rm for(int i=1; i<numSz+1; i++){

since i can be found from the state

```cpp
class Solution {
public:
    int maximumANDSum(vector<int>& nums, int numSlots) {
        int numSz=nums.size();
        //nums.insert(nums.begin(),0); seems work better for matrix dp
        int slotSz=numSlots;
        
        vector<vector<int>> dp(numSz+1, vector<int>(pow(3,slotSz), INT_MIN));
        
        dp[0][0]=0;
        
        for(int state=1; state<pow(3,slotSz); state++){//state should from 1 to make i start from 1
            int i=getNumCnt(state);//ith
            if(i>numSz) continue;//be careful, 222+1-> 1000
            int num=nums[i-1];
            for(int slot=0; slot<slotSz; slot++){//slot idx
                if(numCntInSlot(state,slot)>=1){
                    dp[i][state] = max(dp[i][state], dp[i-1][state - pow(3,slot)] + (num&(slot+1)));//*& priority lower than +*
                }
            }
        }
        
        return *max_element(dp.back().begin(), dp.back().end());
    }
    
    int getNumCnt(int state){
        int res=0;
        while(state>0){
            res+=state%3;
            state/=3;
        }
        return res;
    }
    
    int numCntInSlot(int state, int slot){
        for(int i=0; i<slot;i++){
            state/=3;
        }
        return state%3;
    }
};
```