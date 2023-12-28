# Number of Good Leaf Nodes Pairs

#: 1530
Difficult: Medium
Tags: DFS, Tree
link: https://leetcode.com/problems/number-of-good-leaf-nodes-pairs/description/
Priority: Low
Created time: June 27, 2023 11:44 PM
Last edited time: December 6, 2023 3:58 PM
source: HuaHua, ticktokFreq

You are given the `root` of a binary tree and an integer `distance`. A pair of two different **leaf** nodes of a binary tree is said to be good if the length of **the shortest path** between them is less than or equal to `distance`.

Return *the number of good leaf node pairs* in the tree.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/07/09/e1.jpg](https://assets.leetcode.com/uploads/2020/07/09/e1.jpg)

```
Input: root = [1,2,3,null,4], distance = 3
Output: 1
Explanation: The leaf nodes of the tree are 3 and 4 and the length of the shortest path between them is 3. This is the only good pair.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/07/09/e2.jpg](https://assets.leetcode.com/uploads/2020/07/09/e2.jpg)

```
Input: root = [1,2,3,4,5,6,7], distance = 3
Output: 2
Explanation: The good pairs are [4,5] and [6,7] with shortest path = 2. The pair [4,6] is not good because the length of ther shortest path between them is 4.

```

**Example 3:**

```
Input: root = [7,1,4,6,null,5,3,null,null,null,null,null,2], distance = 3
Output: 1
Explanation: The only good pair is [2,5].

```

**Constraints:**

- The number of nodes in the `tree` is in the range `[1, 210].`
- `1 <= Node.val <= 100`
- `1 <= distance <= 10`

```cpp
//copied from ans to save time
//https://leetcode.com/problems/number-of-good-leaf-nodes-pairs/
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
    int ans = 0;
    vector<int> f(TreeNode * curr,int dis){

        if(curr==nullptr){
            return {};
        }

        if(!curr->left and !curr->right){
            return {1};
        }

        auto left = f(curr->left,dis);
        auto right = f(curr->right,dis);

        if(!left.empty() and !right.empty()){
            vector<int>ret;
            sort(right.begin(),right.end());
            for(auto iter : left){
                int pos = upper_bound(right.begin(),right.end(),dis-iter)-right.begin();
                ans = ans + pos;
            }
        }

        for(int i = 0;i<right.size();i++){
            right[i]++;
        }

        for(auto iter : left){
            right.push_back(iter+1);
        }

        return right;
    }

    int countPairs(TreeNode* root, int distance) {
        f(root,distance);
        return ans;
    }
};

/*
root
distance

leaf node pair is good: length of the shortest path <= distance
return: num of good leaf node pairs
================================
num of node: 1~2e10
val: 1~100
distance: 1~10
================================
val doesn't affect ressult
shortest path alwasy leave to lca to leave
================================
for each node
  left -> 1,2,3
  right -> 2,2
  combine left and right to calcu
    e.g. dist=4
    then 1-2 1-2 2-2 2-2
  return set of len from node to its leaves
*/
```