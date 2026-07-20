> Interesting terminology: https://news.ycombinator.com/item?id=2244201

My prev notes: https://hashdefine.netlify.app/dsa/dp

## 1D DP
- Fibonacci Sequence
- Climbing Stairs
- Min Cost Climbing Stairs - interesting alt state where cost is included in current state
- Frog Jump
- Frog Jump with K Distance - use `for` loop till `k` (space optimization in worst case (`k = n`)  becomes optional)
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
  - variation when array has `0` e.g. `[0,0,1]` - don't stop in middle and return `2`, `1`, or `0` in base cases ([video](https://youtu.be/zoilQD1kYSg))
- Count partitions with given difference - `S = (T - D) / 2`
- 0/1 Knapsack - ultimate optimization of single array right to left filling
- [Coin Change](https://leetcode.com/problems/coin-change/submissions/) - interesting base cases and pick condition. TC = `exponential`, SC = `O(target)`
- [Target Sum](https://leetcode.com/problems/target-sum/submissions/) - either solve by plus and minus (additional base case) or using previous problem of `T - D / 2` as this is equivalent to that!
- [Coin Change II](https://leetcode.com/problems/coin-change-ii/submissions/) - a little change in base case since we're counting ways here and not min no. of coins unlike Coin Change
- Unbounded Knapsack - similar to mininum coins problem where we can steal an item multiple times (base case is no. of times last item can be stolen multiplied by its value)
- Rod Cutting Problem - similar to unbounded knapsack (in base case rod length is `1` so `N` pieces possible, hence `N * value[0]`)
