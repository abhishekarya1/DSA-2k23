**Assign Cookies**: ([link](https://leetcode.com/problems/assign-cookies/)) sort both and then compare from left to right (or right to left). The idea is to assign each cookie to a child with min possible greed that satisfies it. If we assign child `1` to `4` in `2 3 4` instead of `2`, we won't be able to assign child `4` any cookies, so that won't be optimal.
- same problem: [Maximum Matching of Players With Trainers](https://leetcode.com/problems/maximum-matching-of-players-with-trainers/)

**Fractional Knapsack Problem**: calc value/weight ratio and add items in decreasing order of that ratio until knapsack is full.

**Minimum Number of Coins**: start from the highest denomination coin and keep adding it to reach amount (or subtract from amount to reach `0`), move to lower denomination and repeat.


