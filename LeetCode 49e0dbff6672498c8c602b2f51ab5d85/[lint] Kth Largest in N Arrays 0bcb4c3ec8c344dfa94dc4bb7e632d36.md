# [lint] Kth Largest in N Arrays

#: 543
Difficult: Hard
Tags: PQ
link: https://www.lintcode.com/problem/kth-largest-in-n-arrays
Priority: Low
Status: CalcuComplexity
Created time: August 22, 2022 1:00 AM
Last edited time: October 22, 2023 2:08 PM
practicedTimes: 1
source: jiuzhang, 算高TwoPointers&PQ

Find K-th largest element in N arrays.

Example:
Example 1:

```
Input:
k=3, [[9,3,2,4,7],[1,2,3,4,8]]
Output:
7
Explanation:
the 3rd largest element is 7

```

Example 2:

```
Input:
k = 2, [[9,3,2,4,8],[1,2,3,4,2]]
Output:
8
Explanation:
the 1st largest element is 9, 2nd largest element is 8, 3rd largest element is 4 and etc.
```

总结:见到数组先排序 - 九章怎么说，其实直接一个PQ就解决了。两个复杂度有区别。

// from 九章
用什么数据结构?
• Answer:堆
方法:
• 把N个数组先排序
• 排序过后把每个数组最大的数字放入堆里面
• 然后堆里面只维护前k个元素
• 堆pop k次得到答案。
• 时间复杂度N*Len*Log(len)+K*logN (len 是平均每个数组的长度)
//其实直接用PQ
N*Len*logK

```cpp
struct Node {
    int value;
    int from_id;
    int index;
    Node(int _v, int _id, int _i):
        value(_v), from_id(_id), index(_i) {}

    bool operator < (const Node & obj) const {
        return value < obj.value;
    }
};

class Solution {
public:
    /**
     * @param arrays a list of array
     * @param k an integer
     * @return an integer, K-th largest element in N arrays
     */
    int KthInArrays(vector<vector<int>>& arrays, int k) {
        // Write your code here
        priority_queue<Node> queue;

        int n = arrays.size();
        for (int i = 0; i < n; ++i) {
            sort(arrays[i].begin(), arrays[i].end());

            if (arrays[i].size() > 0) {
                int from_id = i;
                int index = arrays[i].size() - 1;
                int value = arrays[i][index];
                queue.push(Node(value, from_id, index));
            }
        }

        for (int i = 0; i < k; ++i) {
            Node temp = queue.top();
            queue.pop();
            int value = temp.value;
            int from_id = temp.from_id;
            int index = temp.index;

            if (i == k - 1)
                return value;

            if (index > 0) {
                index --;
                value = arrays[from_id][index];
                queue.push(Node(value, from_id, index));
            }
        }
    }
};
```