# LIS - Longest Increasing Subsequence

Tags: Array, String, patienceSort

```cpp
int lengthOfLIS(vector<int>& nums) {
        int n=nums.size(); if(n==0) {return 0;}
        vector<int> inc;
        for(auto num: nums){
            auto pos=lower_bound(inc.begin(), inc.end(), num);
            if(pos==inc.end()){inc.push_back(num);}
            else{
                *pos=num;
            }
        }
        return inc.size();
    }
```