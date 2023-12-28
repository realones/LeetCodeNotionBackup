# [lint] Segment Tree Modify

#: 203
Difficult: Medium
Tags: SegmentTree_idx
link: https://www.lintcode.com/problem/203/
Priority: Low
Created time: August 28, 2022 5:57 PM
Last edited time: October 27, 2023 4:50 PM
practicedTimes: 1
source: jiuzhang, 算高SegmentTree

Description

For a `Maximum Segment Tree`, which each node has an extra value `max` to store the maximum value in this node's interval.

Implement a `modify` function with three parameter `root`, `index` and `value` to change the node's change the node's value with ***[start, end] = [index, index]*** to the new given value in segment tree with root `root`. Guarantee every node in segment tree still has the **max** attribute with the correct value after change.

# 

We suggest you finish problem [Segment Tree Build](http://www.lintcode.com/problem/segment-tree-build/) and [Segment Tree Query](http://www.lintcode.com/problem/segment-tree-query/) first.

Example

**Example 1:**

```
Input："[1,4,max=3][1,2,max=2][3,4,max=3][1,1,max=2][2,2,max=1][3,3,max=0][4,4,max=3]",2,4
Output："[1,4,max=4][1,2,max=4][3,4,max=3][1,1,max=2][2,2,max=4][3,3,max=0][4,4,max=3]"
Explanation：
For segment tree:

	                      [1, 4, max=3]
	                    /                \
	        [1, 2, max=2]                [3, 4, max=3]
	       /              \             /             \
	[1, 1, max=2], [2, 2, max=1], [3, 3, max=0], [4, 4, max=3]

if call modify(root, 2, 4), we can get:

	                      [1, 4, max=4]
	                    /                \
	        [1, 2, max=4]                [3, 4, max=3]
	       /              \             /             \
	[1, 1, max=2], [2, 2, max=4], [3, 3, max=0], [4, 4, max=3]

```

**Example 2:**

```
Input："[1,4,max=3][1,2,max=2][3,4,max=3][1,1,max=2][2,2,max=1][3,3,max=0][4,4,max=3]",4,0
Output："[1,4,max=4][1,2,max=4][3,4,max=0][1,1,max=2][2,2,max=4][3,3,max=0][4,4,max=0]"
Explanation：
For segment tree:

	                      [1, 4, max=3]
	                    /                \
	        [1, 2, max=2]                [3, 4, max=3]
	       /              \             /             \
	[1, 1, max=2], [2, 2, max=1], [3, 3, max=0], [4, 4, max=3]
if call modify(root, 4, 0), we can get:

	                      [1, 4, max=2]
	                    /                \
	        [1, 2, max=2]                [3, 4, max=0]
	       /              \             /             \
	[1, 1, max=2], [2, 2, max=1], [3, 3, max=0], [4, 4, max=0]

```

Challenge

Do it in `O(h)` time, h is the height of the segment tree.

Modify tree:

字诀:

- 自上而下递归查找
- 自下而上回溯更新

以数组下标来建立线段树

```cpp
/**
 * Definition of SegmentTreeNode:
 * class SegmentTreeNode {
 * public:
 *     int start, end, max;
 *     SegmentTreeNode *left, *right;
 *     SegmentTreeNode(int start, int end, int max) {
 *         this->start = start;
 *         this->end = end;
 *         this->max = max;
 *         this->left = this->right = NULL;
 *     }
 * }
 */

class Solution {
public:
    /**
     * @param root: The root of segment tree.
     * @param index: index.
     * @param value: value
     * @return: nothing
     */
    void modify(SegmentTreeNode *root, int index, int value) {
        //find leaf
        if(root->start==index && root->end==index){
            root->max=value;
            return;
        }
        //contain
        int mid=root->start+(root->end - root->start)/2;
        if(index<=mid){
            modify(root->left, index, value);
        }
        else if(index>mid){
            modify(root->right, index, value);
        }
        root->max=max(root->left->max, root->right->max);
    }
};
```