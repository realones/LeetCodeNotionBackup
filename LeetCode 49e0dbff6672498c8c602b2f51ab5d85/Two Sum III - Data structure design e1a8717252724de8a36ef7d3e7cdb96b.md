# Two Sum III - Data structure design

#: 170
Difficult: Easy
Tags: 2pt_DiffDir
link: https://leetcode.com/problems/two-sum-iii-data-structure-design/
Priority: Pass
Status: More Solution
Created time: August 6, 2022 11:59 PM
Last edited time: October 19, 2023 9:28 PM
practicedTimes: 1
source: jiuzhang, 算初TwoPointers

Design a data structure that accepts a stream of integers and checks if it has a pair of integers that sum up to a particular value.

Implement the `TwoSum` class:

- `TwoSum()` Initializes the `TwoSum` object, with an empty array initially.
- `void add(int number)` Adds `number` to the data structure.
- `boolean find(int value)` Returns `true` if there exists any pair of numbers whose sum is equal to `value`, otherwise, it returns `false`.

**Example 1:**

```
Input
["TwoSum", "add", "add", "add", "find", "find"]
[[], [1], [3], [5], [4], [7]]
Output
[null, null, null, null, true, false]

Explanation
TwoSum twoSum = new TwoSum();
twoSum.add(1);   // [] --> [1]
twoSum.add(3);   // [1] --> [1,3]
twoSum.add(5);   // [1,3] --> [1,3,5]
twoSum.find(4);  // 1 + 3 = 4, return true
twoSum.find(7);  // No two integers sum up to 7, return false

```

**Constraints:**

- `105 <= number <= 105`
- `231 <= value <= 231 - 1`
- At most `104` calls will be made to `add` and `find`.

set || sort two pointers diff direction

```cpp
class TwoSum {
public:
    unordered_multiset<long> st;
    
    TwoSum() {
        
    }
    
    void add(int number) {
        st.insert(number);
    }
    
    bool find(int value) {
        for(auto num: st){
            int num2=value-num;
            if(num2==num){
                if(st.count(num2)>=2) return true;
            }
            else if(st.count(num2)) return true;
        }
        return false;
    }
};

/**
 * Your TwoSum object will be instantiated and called as such:
 * TwoSum* obj = new TwoSum();
 * obj->add(number);
 * bool param_2 = obj->find(value);
 */
```