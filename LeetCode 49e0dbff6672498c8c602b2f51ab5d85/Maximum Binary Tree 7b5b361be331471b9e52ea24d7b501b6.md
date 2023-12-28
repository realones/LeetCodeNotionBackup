# Maximum Binary Tree

#: 654
Difficult: Medium
Tags: Divide and Conquer, Tree, monotonic, stack
link: https://leetcode.com/problems/maximum-binary-tree/
Priority: High
Created time: September 6, 2022 7:26 PM
Last edited time: October 28, 2023 2:35 PM
practicedTimes: 1
source: HuaHua, jiuzhang, 算高Heap&Stack

You are given an integer array `nums` with no duplicates. A **maximum binary tree** can be built recursively from `nums` using the following algorithm:

1. Create a root node whose value is the maximum value in `nums`.
2. Recursively build the left subtree on the **subarray prefix** to the **left** of the maximum value.
3. Recursively build the right subtree on the **subarray suffix** to the **right** of the maximum value.

Return *the **maximum binary tree** built from* `nums`.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/12/24/tree1.jpg](https://assets.leetcode.com/uploads/2020/12/24/tree1.jpg)

```
Input: nums = [3,2,1,6,0,5]
Output: [6,3,5,null,2,0,null,null,1]
Explanation: The recursive calls are as follow:
- The largest value in [3,2,1,6,0,5] is 6. Left prefix is [3,2,1] and right suffix is [0,5].
    - The largest value in [3,2,1] is 3. Left prefix is [] and right suffix is [2,1].
        - Empty array, so no child.
        - The largest value in [2,1] is 2. Left prefix is [] and right suffix is [1].
            - Empty array, so no child.
            - Only one element, so child is a node with value 1.
    - The largest value in [0,5] is 5. Left prefix is [0] and right suffix is [].
        - Only one element, so child is a node with value 0.
        - Empty array, so no child.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/12/24/tree2.jpg](https://assets.leetcode.com/uploads/2020/12/24/tree2.jpg)

```
Input: nums = [3,2,1]
Output: [3,null,2,null,1]

```

**Constraints:**

- `1 <= nums.length <= 1000`
- `0 <= nums[i] <= 1000`
- All integers in `nums` are **unique**.

解决思路

1. 扫描线从上往下，递归。但是max高度指定，不好弄。（解法一）
2. 暴力解法，找重复计算，发现同一段区间会扫描了多次，同一个区间扫一次应该就可以构建一个局部解
3. 思考类数学归纳法推导，前K个扫过了，现在扫K+1个，
4. if 比之前的所有值大 -> new root，会怎么影响k+2，k+x，会不会一直是new root
if not -> 会在当前root的右子树里，插入到所有root右边比k+1的值小的node上面，（解法二）
5. 同时多列列找到的规律和推导公式
6. decrease总是会进入当前的右子树
increase会插入为新的sub root，甚至成为root，同时固定住这个sub root左边的nodes
所以decrease不着急放到tree里，暂时是虚的，可以cache起来，用个stack（解法三）
整理一下思路和建模，虚（cache）和实（直接放到局部解里）思考怎么关联

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        nums.push_back(INT_MAX);
        stack<TreeNode*> stk;//decreasing order
        for(auto num: nums){
            TreeNode* node = new TreeNode(num);
            TreeNode* last = NULL;
            while(!stk.empty() && stk.top()->val<num){
                TreeNode* cur=stk.top(); stk.pop();
                cur->right=last;
                last=cur;
            }
            node->left=last;
            stk.push(node);
        }
        return stk.top()->left;
    }
};
```

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* constructMaximumBinaryTree(vector<int>& A) {
        // 单调递减栈
        A.push_back(INT_MAX);
        stack<TreeNode*> stk;

        for(auto a: A){
            //if smaller than top or empty, push in
            //else find a parent for top: 1. if empty, 2. find the smaller one
            TreeNode *nxt=new TreeNode(a);
            while(!stk.empty() && stk.top()->val<a){
                TreeNode *cur=stk.top(); stk.pop();
                if(stk.empty()){nxt->left=cur;}
                else{
                    TreeNode* pre=stk.top();
                    if(pre->val<nxt->val){pre->right=cur;}
                    else{nxt->left=cur;}
                }
            }

            stk.push(nxt);
        }

        return stk.top()->left;
    }
};
```