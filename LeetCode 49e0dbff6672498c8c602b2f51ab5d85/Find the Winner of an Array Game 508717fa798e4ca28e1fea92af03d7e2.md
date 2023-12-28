# Find the Winner of an Array Game

#: 1535
Difficult: Medium
Tags: Array, queue, simulation
link: https://leetcode.com/problems/find-the-winner-of-an-array-game/
Priority: High
Status: CalcuComplexity, No Idea At First
Created time: November 7, 2023 6:07 PM
Last edited time: November 7, 2023 6:14 PM
source: leetcode_daily

Given an integer array `arr` of **distinct** integers and an integer `k`.

A game will be played between the first two elements of the array (i.e. `arr[0]` and `arr[1]`). In each round of the game, we compare `arr[0]` with `arr[1]`, the larger integer wins and remains at position `0`, and the smaller integer moves to the end of the array. The game ends when an integer wins `k` consecutive rounds.

Return *the integer which will win the game*.

It is **guaranteed** that there will be a winner of the game.

**Example 1:**

```
Input: arr = [2,1,3,5,4,6,7], k = 2
Output: 5
Explanation: Let's see the rounds of the game:
Round |       arr       | winner | win_count
  1   | [2,1,3,5,4,6,7] | 2      | 1
  2   | [2,3,5,4,6,7,1] | 3      | 1
  3   | [3,5,4,6,7,1,2] | 5      | 1
  4   | [5,4,6,7,1,2,3] | 5      | 2
So we can see that 4 rounds will be played and 5 is the winner because it wins 2 consecutive games.

```

**Example 2:**

```
Input: arr = [3,2,1], k = 10
Output: 3
Explanation: 3 will win the first 10 rounds consecutively.

```

**Constraints:**

- `2 <= arr.length <= 105`
- `1 <= arr[i] <= 106`
- `arr` contains **distinct** integers.
- `1 <= k <= 109`

comment: time complexity calculation difficulty, simulation mathematical modeling difficulty

Solution 1: 

Comment: why the while loop T is O(n)?

```jsx
class Solution {
public:
    int getWinner(vector<int> nums, int k) {
        int n=nums.size();
        //corner case
        if(k>=n) return *max_element(nums.begin(), nums.end());

        //simulate
        int a=nums[0];
        queue<int> q;
        for(int i=1; i<n; i++){
            q.push(nums[i]);
        }
        //process
        int winStreak=0;
        while(true){//O(n)?
            int b=q.front(); q.pop();
            if(a>b){
                winStreak++;
                q.push(b);
            }
            else{
                winStreak=1;
                q.push(a);
                a=b;
            }
            if(winStreak==k) return a;
        }
        return -1;
    }
};

/*
nums:
    len: 2~1e5
    val: 1~1e5, distinct
k: 1~1e9

================================
Intui 1: seems TLE
find first element that can last k length?

dp[i]: how many smaller elem on its rightside

dp[i] = 
    larger than right: dp[i+1] + 1
    smaller than right: 0

then for each i, find first i that dp[i] >= k
*/
```

Solution 2: no queue

```jsx
class Solution {
public:
    int getWinner(vector<int> nums, int k) {
        int n=nums.size();
        //corner case
        if(k>=n) return *max_element(nums.begin(), nums.end());

        //simulate
        int a=nums[0];
        //process
        int winStreak=0;
        for(int i=1;i<n;i++){//O(n)?
            int b=nums[i];
            if(a>b){
                winStreak++;
            }
            else{
                winStreak=1;
                a=b;
            }
            if(winStreak==k) return a;
        }
        return a;//
    }
};

/*
Each player that is not maxElement has two possibilities:

They come after maxElement in arr.
They come before maxElement in arr.
If a player comes after maxElement, they will not play any rounds in our simulation, since we immediately terminate upon finding maxElement.

If a player comes before maxElement and loses, they will move to the back of the line behind maxElement. This means they will never appear in the simulation again, because maxElement will play before them, and we immediately terminate the simulation once maxElement plays.

Thus, in our simulation, when a player loses, they never play again. That means we don't actually need the queue to maintain their positions at all! We can simply use a for loop to iterate over the opponents while implementing the same simulation.
*/
```