- refer Samsung notes

Two templates:
1. Pick/NotPick - subsequences
2. FOR loop - permutations & combinations, there is no base case as such, not-pick step is absent

Base cases: In Pick/NotPick it is `i == arr.size()`. In FOR loop approach base case isn't required since FOR loop range takes care of array traversal bounds

Whenever we require to remove duplicates (uniqueness), we `sort` and then use the FOR loop approach. Ex - ComboSum2, SubsetSum2. 

Sort condition: `if(i > p && arr[i] == arr[i - 1]) continue;`. 

We can further optimize the FOR loop using `if(arr[i] > target) break;` (if current element can't be part of the answer then remaining array can't be too; since array is sorted).

Min Stack: retrieves minimum element in `O(1)` time. If we have a new minimum push `2*element - min` into the stack, it is guaranteed that the value we are pushing is less than element we are pushing (also our new minimum) and this position is where we encountered a new minimum. While popping, we need to make sure that our minimum changes when we are popping the minimum element from the stack so we check for change point and then modify minimum accordingly.
