# Maximum Total Beauty of the Gardens

#: 2234
Difficult: Hard
Tags: 2pt, Array, BS_Idx, BS_lower_upper_bound, Greedy, Sort
link: https://leetcode.com/problems/maximum-total-beauty-of-the-gardens
Priority: High
Status: CalcuComplexity, Dig More, HardToImplement, No Idea At First, NotUnderstand, toRevisit
Created time: November 17, 2023 1:28 AM
Last edited time: November 18, 2023 10:33 PM
practicedTimes: 1
source: ticktokFreq

Alice is a caretaker of `n` gardens and she wants to plant flowers to maximize the total beauty of all her gardens.

You are given a **0-indexed** integer array `flowers` of size `n`, where `flowers[i]` is the number of flowers already planted in the `ith` garden. Flowers that are already planted **cannot** be removed. You are then given another integer `newFlowers`, which is the **maximum** number of flowers that Alice can additionally plant. You are also given the integers `target`, `full`, and `partial`.

A garden is considered **complete** if it has **at least** `target` flowers. The **total beauty** of the gardens is then determined as the **sum** of the following:

- The number of **complete** gardens multiplied by `full`.
- The **minimum** number of flowers in any of the **incomplete** gardens multiplied by `partial`. If there are no incomplete gardens, then this value will be `0`.

Return *the **maximum** total beauty that Alice can obtain after planting at most* `newFlowers` *flowers.*

**Example 1:**

```
Input: flowers = [1,3,1,1], newFlowers = 7, target = 6, full = 12, partial = 1
Output: 14
Explanation: Alice can plant
- 2 flowers in the 0th garden
- 3 flowers in the 1st garden
- 1 flower in the 2nd garden
- 1 flower in the 3rd garden
The gardens will then be [3,6,2,2]. She planted a total of 2 + 3 + 1 + 1 = 7 flowers.
There is 1 garden that is complete.
The minimum number of flowers in the incomplete gardens is 2.
Thus, the total beauty is 1 * 12 + 2 * 1 = 12 + 2 = 14.
No other way of planting flowers can obtain a total beauty higher than 14.

```

**Example 2:**

```
Input: flowers = [2,4,5,3], newFlowers = 10, target = 5, full = 2, partial = 6
Output: 30
Explanation: Alice can plant
- 3 flowers in the 0th garden
- 0 flowers in the 1st garden
- 0 flowers in the 2nd garden
- 2 flowers in the 3rd garden
The gardens will then be [5,4,5,5]. She planted a total of 3 + 0 + 0 + 2 = 5 flowers.
There are 3 gardens that are complete.
The minimum number of flowers in the incomplete gardens is 4.
Thus, the total beauty is 3 * 2 + 4 * 6 = 6 + 24 = 30.
No other way of planting flowers can obtain a total beauty higher than 30.
Note that Alice could make all the gardens complete but in this case, she would obtain a lower total beauty.

```

**Constraints:**

- `1 <= flowers.length <= 105`
- `1 <= flowers[i], target <= 105`
- `1 <= newFlowers <= 1010`
- `1 <= full, partial <= 105`

comment: algorithm看了wisdom才明白，然后实现花了很久总是有错误。

Solution - votrubac

todo: calcu complexity, seems TLE but passed

```cpp
using ll=long long;

class Solution {
public:
    long long maximumBeauty(vector<int>& fl, long long newFlowers, int target, int full, int partial) {
        ll n = fl.size();
        sort(begin(fl), end(fl), greater<int>());
        //p1: full gardens
        ll p1 = 0;
        //make as many gardens to be full as possible, left to right
        for (; p1 < n; ++p1) { //T: O(n)
            if (target - fl[p1] > newFlowers)
                break;
            newFlowers -= max(0, target - fl[p1]);
        }
        //corner case - if all can be full: either all full, or execpt one == target-1
        //todo check if without newFlowers all already full
        if (p1 == n)
            return max(n * full, (n - 1) * full + (fl.back() < target ? (ll)(target - 1) * partial : full));
        //sum: sum of flowers in garden that <= minF
        ll sum = 0, res = 0;
        //p2: gardens < minF, minF mono inc till target-1
        for (ll minF = fl.back(), p2 = n - 1; minF < target; ) {//O(target)
            while (p2 >= p1 && fl[p2] <= minF) //O(n)
                sum += fl[p2--];
            int needed = (n - p2 - 1) * minF - sum;//needed flowers to make smaller ones to >= minF
            if (needed > newFlowers) {//if flower not enough to make minF
                if (--p1 < 0)//p1 move to left, make p1's full garden non-full
                    break;
                newFlowers += max(0, target - fl[p1]);
            }
            else {
                res = max(p1 * full + minF * partial, res);    
                ++minF;
            }
        }
        return res;
    }
};
/*
from votrubac
*/
```

Solution - wrong ans for some test case, checked thought from wisdomPeak, tried a few hours and failed, sigh

```cpp
using ll=long long;

class Solution {
public:
    long long maximumBeauty(vector<int>& input, long long newFlowers, ll target, ll full, ll partial) {
        vector<ll> flowers(input.begin(), input.end());
        ll n=flowers.size();
        sort(flowers.begin(), flowers.end());

        std::cout<< "flowers" <<" -- "; for (const auto& elem : flowers) { cout << setw(4) << elem << " "; } std::cout<<std::endl;
        std::cout << "newFlowers" << " -- " << newFlowers << std::endl;
        std::cout << "target" << " -- " << target << std::endl;
        std::cout << "full" << " -- " << full << std::endl;
        std::cout << "partial" << " -- " << partial << std::endl;

        //build prefixsum
        vector<ll> prefixSum(flowers.begin(), flowers.end());
        for(ll i=1;i<n;i++){
            prefixSum[i]+=prefixSum[i-1];
        }
        function getPrefixSum = [&](ll l, ll r){return prefixSum[r] - (l==0?0:prefixSum[l-1]);};
        std::cout<< "prefixSum" <<" -- "; for (const auto& elem : prefixSum) { cout << setw(4) << elem << " "; } std::cout<<std::endl;

        //build diff - the additional flowers required to update [0~i] to flat
        vector<ll> diff(flowers.begin(), flowers.end());//mono inc
        for(ll i=0;i<n;i++){
            diff[i] = flowers[i]*(i+1) - getPrefixSum(0,i);
        }
        std::cout<< "diff" <<" -- "; for (const auto& elem : diff) { cout << setw(4) << elem << " "; } std::cout<<std::endl;

        //requiredFlowersToMakeComplete for [i~n-1]
        vector<ll> required(n,0);
        for(ll i=n-1;i>=0;i--){
            if(flowers[i]>=target) continue;
            else{
                required[i] = target-flowers[i] + (i+1<n?required[i+1]:0);
            }
        }
        std::cout<< "required" <<" -- "; for (const auto& elem : required) { cout << setw(4) << elem << " "; } std::cout<<std::endl;
        
        //max idx that diff(idx) <= newFlowers
        function findP = [&](ll r){
            ll l=0;
            while(l<r){
                ll m=l+((long long)r-l+1)/2;
                if(diff[m]>newFlowers) {r=m-1;}//if not qualify
                else {l=m;}
            }
            return r;
        };

        std::cout << "start Processing..." << std::endl;
        //process
        ll res=0;
        for(ll i=-1; i<n; i++){
            std::cout << "###" << std::endl;
            std::cout << "i" << " -- " << i << std::endl;
            if(i>=0 && flowers[i]>=target) break;
            ll leftNewFlowers=newFlowers;
            //flowers[i+1~n-1] is complete
            ll gain1=0;
            if(i==n-1) gain1=0;//all incomp
            else{
                std::cout << "required[i+1]" << " -- " << required[i+1] << std::endl;
                if(required[i+1]>newFlowers) continue;//not enough flowers
                leftNewFlowers -= required[i+1];
                gain1 = (n-i-1) * full;
                if(i==-1){//only have gain1
                    std::cout << "gain1" << " -- " << gain1 << std::endl;
                    res=max(res,gain1); 
                    continue;
                }
            }
            std::cout << "gain1" << " -- " << gain1 << std::endl;

            std::cout << "leftNewFlowers" << " -- " << leftNewFlowers << std::endl;
            //flowers[0~i] is incomplete
            ll gain2=0;
            //if we have enough to update all flowers to (target-1)
            if(i==9){
                std::cout << "@@@@" << std::endl;
                std::cout << "i+1" << " -- " << i+1 << std::endl;
                std::cout << "target-1" << " -- " << target-1 << std::endl;
                std::cout << "getPrefixSum(0,i)" << " -- " << getPrefixSum(0,i) << std::endl;
                std::cout << "leftNewFlowers" << " -- " << leftNewFlowers << std::endl;
            }
            if((i+1)*(target-1) - getPrefixSum(0,i) <= leftNewFlowers){
                std::cout << "have enough to upgrade all to target-1" << std::endl;
                gain2 = (target-1) * partial;
            }
            else{
                ll p=findP(i);//find max p that leftNewFlowers >= diff(p);
                ll minIncomp = (prefixSum[p] + leftNewFlowers)/(p+1);
                std::cout << "p" << " -- " << p << std::endl;
                std::cout << "minIncomp" << " -- " << minIncomp << std::endl;
                gain2 = (ll)minIncomp * partial;
            }
            std::cout << "gain2" << " -- " << gain2 << std::endl;
            res=max(res,gain1+gain2);
            std::cout << "gain1+gain2" << " -- " << gain1+gain2 << std::endl;
        }
        return res;
    }
};
/*
wisdomPeak:
we need to divide final flowers to two groups: incompletes and completes
the best way is to changing larger ones into complete ones
==>
sort flowers, then find i that
    flowers[0~i] is incomplete
    flowers[i+1~n-1] is complete
        newFlowers -= FlowersUsedToCompleteThisGroup
        gain1 = (n-i) * full

subquestion:
    flowers[0~i] is incomplete

case 1:
    we have enough to update all flowers to (target-1)
        gain2: (target-1) * (i+1)
case 2:
    we dont' have enough to update all
    we update from 0~p, [p+1,i] is not updated
    p: can update 0~p to new val, but not with p+1
    =>
    find max P, that
        prefixSum[0,p] + newFlowers >= flowers[p]*(p+1): update 0~p to at least flower[0]
    let diff[p] = flower[p]*(p+1) - prefixSum[p]: the additional flowers required to update [0~p]
        diff[p] is mono inc -> BS to figure out p
    minIncomp = (prefixSum[p] + newFlowers)/(p+1)
    gain2 = minIncomp * partial

algorithm: 
    fori: to divide incompletes and completes - O(n)
        BS to find p - O(logn)
    T: O(nlogn)

Btw: p is mono from larger to small, so the BS finding theratically can be improved to O(n), but we sorted at first, there is no need to improve here.
*/

/*
n gardens

flowers[i] is the number of flowers already planted in the ith garden.
Flowers that are already planted cannot be removed.

A garden is complete if >= target flowers. 

op: planting <= newFlowers flowers.

return max of:
    The number of complete gardens * full.
    +
    The minimum number of incomplete flowers * partial.
================================
flowers:
    len: 1~1e5
    val: 1~1e5
target: 1~1e5
newFlowers: 1~1e10
full, partial: 1~1e5
================================
assign "newFlower" number into vector "flowers", each elem at most == target
return: 
    max of ((cnt of elem == target) * full + (min of elem < target) * partial)
================================

*/
```