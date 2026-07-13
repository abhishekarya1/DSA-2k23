> Interesting terminology: https://news.ycombinator.com/item?id=2244201

My prev notes: https://hashdefine.netlify.app/dsa/dp

## 1D DP
- Fibonacci Sequence
- Climbing Stairs
- Min Cost Climbing Stairs - interesting state
- Frog Jump
- Frog Jump with K Distance - use `for` loop till `k`
- House Robber - max sum of non-adjacent elements
- House Robber II - circular form of the above

## 2D DP
- Ninja's Training

## DP on Subsequences
Subset sum equals target
- Variation when array has negatives - use `vector<unordered_map<int, bool>>` as we check and use only truths in the previous row anyways so we don't need all sum entries
- [Partition equal subset sum](https://leetcode.com/problems/partition-equal-subset-sum/submissions/) - `S = T/2`
- Partition a Set into 2 Subsets with Minimum Absolute Sum Difference - calc `S1` as `T - S2` in tabulation's last row
- Count subsets with sum `k` - return `1` in base cases
  - variation when array has `0` e.g. `[0,0,1]` - don't stop in middle and return `1` or `2` in base cases
- Count partitions with given difference - `S = (T - D) / 2`
- 0/1 Knapsack - ultimate optimization of single array right to left filling
- [Coin Change](https://leetcode.com/problems/coin-change/submissions/) - interesting base case and pick condition
- [Target Sum](https://leetcode.com/problems/target-sum/submissions/) - either solve by plus and minus (additional base case) or using previous problem of `T - D / 2` as this is equivalent to that!
