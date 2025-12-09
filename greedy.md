Patterns: https://chatgpt.com/share/693856bc-ecd8-800e-95d1-ef32cba717ef

Greedy Algorithms can be classified broadly among these known patterns:
- Resource Allocation
- Change-making
- Pick items based on ratio (value/weight, profit/time, etc.)
- Interval-based: merging intervals, activity selection, scheduling

---

**Assign Cookies**: ([link](https://leetcode.com/problems/assign-cookies/)) sort both and then compare from left to right (or right to left). The idea is to satisfy each child with the smallest cookie that can meet their greed. If we assign child `1` to cookie `4` in `2 3 4` instead of `2`, we won't be able to assign child `4` any cookies (if they exist), so that won't be optimal.
- sorting both and comparing like that works because the problem has a "monotonic" structure — children with higher greed can only be satisfied by cookies that are the same or larger. After sorting, once a cookie fails to satisfy a child, it will fail for all later children; once it succeeds, you safely move on.
- same problem: [Maximum Matching of Players With Trainers](https://leetcode.com/problems/maximum-matching-of-players-with-trainers/)

**Fractional Knapsack Problem**: calc value/weight ratio and add items in decreasing order of that ratio until knapsack is full.

**Minimum Number of Coins**: start from the highest denomination coin and keep adding it to reach amount (or subtract from amount to reach `0`), move to lower denomination and repeat. This greedy method assumes the given target amount can be formed using the given denominations and we're given a [canonical](https://chatgpt.com/s/t_693444afa5ec8191ab55a45434c1d29e) coin system.

**Lemonade Change**: ([link](https://leetcode.com/problems/lemonade-change/)) keep track of how many `$5` and `$10` bills you have, and for each customer give change using the largest bills possible while ensuring you never go negative. If at any point you cannot provide exact change, return `false`.

**Valid Parenthesis String**: ([link](https://leetcode.com/problems/valid-parenthesis-string/)) track `minOpen` and `maxOpen` for all chars, these basically denote a range of open counts if `*` is considered `)`, `''`, or `(`. At every char check both the counts `< 0` and decide invalid or convert/relax `*`. At the end, check if we've closed all open parenthesis in atleast one interpretation of `*` i.e. `minOpen == 0`, otherwise even the "most closed" case still leaves you with leftover opens.

[intervals notes](https://github.com/abhishekarya1/DSA-2k23/blob/main/arrays.md#intervals)

**Number of Meetings Possible to Attend** / **N Meetings in one Room**: the goal is to find out the max number of meetings one person can attend (or max meetings a room can accomodate). This is nothing but finding [max NOI](https://leetcode.com/problems/non-overlapping-intervals/).

**Jump Game**: ([link](https://leetcode.com/problems/jump-game/)) track `maxReachable` index and update it on every max `i + nums[i]`, if we encounter an index that's greater than it then we can't proceed further (end is unreachable), return `false`.

**Jump Game II**: ([link](https://leetcode.com/problems/jump-game-ii/)) same as above but track the end of the current range and reaching it means we've exhausted every index that can be reached with the current number of jumps, so if we want to move forward at all, we are forced to increase the jump count. Hence track `currentEnd` along with `maxReachable` for this. ([clarification](https://chatgpt.com/s/t_69370638565881918c571011b4d11c88))

**Minimum number of Rooms / Platforms Required** (Meeting Rooms II): ([link](https://takeuforward.org/data-structure/minimum-number-of-platforms-required-for-a-railway/)) sort both start and end times and look for start times `<=` a given end time, update `roomCnt++` for each such start time, and update pointer to start time (as it doesn't matter anymore since we added a room for it), otherwise just update pointer to end time (as it doesn't matter anymore since we're already past it). This is nothing but max OI.

**Job Scheduling** with Deadlines and Profit: simply picking max profits first won't work e.g. `{ (A, 1, 10), (B, 2, 50) }` has max profit as `60` and not `50` (if we pick job `B` first and it takes one time unit). Sort jobs in desc order of profit, then dump each job into the last free slot before its deadline, by doing reverse traversal on a `slots[maxDeadline + 1]` array from its deadline. Update total profit on a new entry and put job id in the slot for getting sequence. ([explanation](https://chatgpt.com/share/6938610b-79c8-800e-afb2-56c86046d753))

**Shortest Job First (SJF)** for CPU Scheduling: sort jobs in asc order, and the total waiting time for a given job will be accumulated time of all jobs executed prior to that job. Total waiting time will be the sum of prefix sums of all elements.


