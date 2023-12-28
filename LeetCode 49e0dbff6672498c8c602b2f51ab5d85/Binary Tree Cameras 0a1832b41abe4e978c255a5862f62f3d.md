# Binary Tree Cameras

#: 968
Difficult: Hard
Tags: DFS, DP, Greedy, Tree
link: https://leetcode.com/problems/binary-tree-cameras/description/
Priority: High
Status: Dig More, HardToImplement, No Idea At First, toSummarize
Created time: August 13, 2023 10:03 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: House Robber III (House%20Robber%20III%203c624936740141239137d7cddd7f397f.md), Distribute Coins in Binary Tree (Distribute%20Coins%20in%20Binary%20Tree%20622cbfc2f2824e46896afaa9354a092b.md)

You are given the `root` of a binary tree. We install cameras on the tree nodes where each camera at a node can monitor its parent, itself, and its immediate children.

Return *the minimum number of cameras needed to monitor all nodes of the tree*.

**Example 1:**

![https://assets.leetcode.com/uploads/2018/12/29/bst_cameras_01.png](https://assets.leetcode.com/uploads/2018/12/29/bst_cameras_01.png)

```
Input: root = [0,0,null,0,0]
Output: 1
Explanation: One camera is enough to monitor all nodes if placed as shown.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2018/12/29/bst_cameras_02.png](https://assets.leetcode.com/uploads/2018/12/29/bst_cameras_02.png)

```
Input: root = [0,0,null,0,null,0,null,null,0]
Output: 2
Explanation: At least two cameras are needed to monitor all nodes of the tree. The above image shows one of the valid configurations of camera placement.

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 1000]`.
- `Node.val == 0`

My comment:

stateMachine + greedy → action sequence has priority

**Solutions**:

Solution wrong - failed test case: [0,0,null,0,0]

get root state [put_camera, no_camera] from left and right children

没想到要child告诉parent需不需要camera，也没考虑连续两个no camera：1，0，0，1的情况

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
using ii=pair<int,int>;

class Solution {
public:
    int minCameraCover(TreeNode* root) {
        if(!root) return 0;
        if(!root->left && !root->right) return 1;
        auto [rootc,rootn]=recur(root);
        return min(rootc,rootn);
    }

    //give [camera res, no_camera res] for root
    ii recur(TreeNode* root){
        //init
        if(!root) return {1000,0};
        if(!root->left && !root->right) return {1,1};

        auto [lc, ln]=recur(root->left);
        auto [rc, rn]=recur(root->right);
        int rootc=min(lc,ln)+min(rc,rn)+1;
        int rootn=min({
                    lc+rc,
                    lc+rn,
                    ln+rc,
                });
        std::cout << "rootc" << " -- " << rootc << std::endl;
        std::cout << "rootn" << " -- " << rootn << std::endl;
        return {rootc,rootn};
    }
};

/*
================================
if arr:
    rob house
        state:
            camera
            no_camera
        action:
            do nothing
            put camera
        combinations
            camera + do_nothing
            camera + put_camera
            no_camera + put_camera
    ==>
    camera = min
                 camera + put_camera (+1)
                 no_camera + put_camera (+1)
    no_camera =  camera + do_nothing (+0)

    return min(camera,no_camera)
================================ 
if tree:
    bottom up - from leaves to root
        recursion

    reuse node val as the dp val
        how to use one int to save two val?
            --option 1: clone tree to some other structure
            --option 2: mod, or split by large number
            --*option 3: mp[TreeNode*, node_res]

    state:
        camera
        no_camera
    action:
        do nothing
        put camera
    combinations
        camera + do_nothing
        camera + put_camera
        no_camera + put_camera
    ==>
    camera = min
                 any_state + put_camera (+1)

    no_camera =  l|r camera + do_nothing (+0)

    return min(camera,no_camera)
*/
```

Solution state machine 1:

idea from wisdom peak, implemented by myself:

hard to implement, difficult to find init value for null and leaves, at last i have to split to situation for l&&r, l, r separately. 

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

//0: uncovered
//1: with camera
//2: covered without camera
using iii=tuple<int,int,int>;

class Solution {
public:
    int minCameraCover(TreeNode* root) {
        auto [s0,s1,s2] = helper(root);
        return min({s0+1,s1,s2});
    }

    iii helper(TreeNode* root){
        if(!root) return {1e7,1e7,1e7};
        if(!root->left && !root->right) return {0,1,1000};
        int s0,s1,s2;
        auto [l0,l1,l2]=helper(root->left);
        auto [r0,r1,r2]=helper(root->right);
        bool l=root->left;
        bool r=root->right;
        if(l&&r){
            s0=l2+r2;
            s1=min(
                    l0+min({r0,r1,r2}),
                    r0+min({l0,l1,l2})
                )+1;
            s2=min(
                    l1+min({r1,r2}),
                    r1+min({l1,l2})
                );
        }
        else if(l){
            s0=l2;
            s1=l0+1;
            s2=l1;
        }
        else{//r
            s0=r2;
            s1=r0+1;
            s2=r1;
        }

        return {s0,s1,s2};
    }
};

/*
my attempt: i think it should be state machine, but thought only two state, failed to break down to cover all states
================================
from wisdom peak
state machine:
state:
    not_covered 0
    covered
        c_has_camera 1
        c_no_camera 2
action:
    affect nxt one
changes:
    not_covered  -> nxt one: c_has_camera
    c_has_camera -> nxt one: c_no_camera
    c_no_camera  -> nxt one: not_covered
seq:
    root to leave?
        incorrect!
            since state not just affected by parents, 
            but also siblings -> can't implemented by linear direction dp iteration
    leaves to root?
        seems workable -> when checking node, both children's states are known
				[***following has priority***]
            - any child *not_covered*,
                node -> *c_has_camera*, res++
            - both child *c_no_camera*,
                node -> *not_covered*, res==
                    greedy(交给上面兜底,你cover上面不如上面cover你实惠)
            - other
                node -> *c_has_camera*, res==
init:
    leaves: greedy let parent to handle to gain best result
        all leaves : *not_covered*
intui:
    这个greedy要double check是正确的
corner case:
    if root end with 0, we should put one more camera in root to cover it
impl:
    recurrsion, each node return [eachState-minCameraCnt]
*/
```

above Improved to:

null’s s2 == 0 makes sense but difficult to come up with.

```cpp
//0: uncovered
//1: with camera
//2: covered without camera
using iii=tuple<int,int,int>;

class Solution {
public:
    int minCameraCover(TreeNode* root) {
        auto [s0,s1,s2] = helper(root);
        return min({s0+1,s1,s2});
    }

    iii helper(TreeNode* root){
        if(!root) return {1e3,1e3,0};
        if(!root->left && !root->right) return {0,1,1e3};
        int s0,s1,s2;
        auto [l0,l1,l2]=helper(root->left);
        auto [r0,r1,r2]=helper(root->right);
        s0=l2+r2;
        s1=min(
                l0+min({r0,r1,r2}),
                r0+min({l0,l1,l2})
            )+1;
        s2=min(
                l1+min({r1,r2}),
                r1+min({l1,l2})
            );
        return {s0,s1,s2};
    }
}
```

solution wisdompeak:

very greedy, to be summarize

```cpp
class Solution {
    // 0: uncovered
    // 1: with camera
    // 2: covered without camera
    int result;
public:
    int minCameraCover(TreeNode* root) 
    {
        result = 0;
        int temp = DFS(root);
        if (temp==0) result++;
        return result;
    }
    
    int DFS(TreeNode* node)
    {
        if (node==NULL) return 2;//same with other solutions
        int left = DFS(node->left);
        int right = DFS(node->right);
        if (left==0 || right==0)
        {
            result++;
            return 1;
        }
        if (left==2 && right==2)
        {
            return 0;
        }
        return 2;
    }
}
```