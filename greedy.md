Patterns: https://chatgpt.com/share/69335b0d-3150-800e-bf69-3b56c9ac608b

**Assign Cookies**: ([link](https://leetcode.com/problems/assign-cookies/)) sort both and then compare from left to right (or right to left). The idea is to satisfy each child with the smallest cookie that can meet their greed. If we assign child `1` to cookie `4` in `2 3 4` instead of `2`, we won't be able to assign child `4` any cookies (if they exist), so that won't be optimal.
- sorting both and comparing like that works because the problem has a "monotonic" structure — children with higher greed can only be satisfied by cookies that are the same or larger. After sorting, once a cookie fails to satisfy a child, it will fail for all later children; once it succeeds, you safely move on.
- same problem: [Maximum Matching of Players With Trainers](https://leetcode.com/problems/maximum-matching-of-players-with-trainers/)

**Fractional Knapsack Problem**: calc value/weight ratio and add items in decreasing order of that ratio until knapsack is full.

**Minimum Number of Coins**: start from the highest denomination coin and keep adding it to reach amount (or subtract from amount to reach `0`), move to lower denomination and repeat. This greedy method assumes the given target amount can be formed using the given denominations and we're given a [canonical](https://chatgpt.com/s/t_693444afa5ec8191ab55a45434c1d29e) coin system.

**Lemonade Change**: ([link](https://leetcode.com/problems/lemonade-change/)) keep track of how many `$5` and `$10` bills you have, and for each customer give change using the largest bills possible while ensuring you never go negative. If at any point you cannot provide exact change, return `false`.

**Valid Parenthesis String**: ([link](https://leetcode.com/problems/valid-parenthesis-string/)) track `minOpen` and `maxOpen` for all chars, these are basically count of opens if `*` is considered `)` and `(` respectively. At every char check both the counts `< 0` and decide and convert/relax `*`. At the end, check if we've closed all open parenthesis i.e. `minOpen == 0`.

