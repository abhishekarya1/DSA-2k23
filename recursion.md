- refer Samsung notes

Two templates:
1. Pick/NotPick - subsequences
2. FOR loop - permutations & combinations, there is no base case as such, not-pick step is absent

Base cases: In Pick/NotPick it is `i == arr.size()`. In FOR loop approach base case isn't required since FOR loop range takes care of array traversal bounds

Whenever we require to remove duplicates (uniqueness), we `sort` and then use the FOR loop approach. Ex - ComboSum2, SubsetSum2. 

Sort condition: `if(i > p && arr[i] == arr[i - 1]) continue;`. 

We can further optimize the FOR loop using `if(arr[i] > target) break;` (if current element can't be part of the answer then remaining array can't be too; since array is sorted).

**2-D**: the main string to be traversed is traevsed by recursion calls, other by FOR loop. Ex - in phone keypad problem, we traverse digits by recursion and run FOR loop for "abc" "def" etc... Another example - combination sum problems are also this way only, there we place 1 element of the array using recursion and using FOR loop we traverse the rest of the array to find pairs for it

### Advanced Problems
**Palindrome Partitioning**: check all partition positions, if a partition's left substring (`place` to `i`) is a palindrome then recur for remaining string, else move ahead `i++` with the same position `place` as parition

**Word Search**: recur on string `word` as soon as the first occurance of the `word[0]` is found, traverse all four directions matching subsequent characters of `word` and if in any direction we reach a dead end (either grid boundaries, non-matching character, or an already visited cell). Edge case - mark visited in grid as `!` because we can traverse the grid and loop back to the same element again and use it again which isn't valid. Make sure to backtrack i.e. unvisit after recursive calls
