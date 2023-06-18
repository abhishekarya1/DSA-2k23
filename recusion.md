- refer Samsung notes

Two templates:
1. Pick/NotPick - subsequences, permutations
2. FOR loop - combinations, there is no base case as such, not-pick step is absent

Base cases: In Pick/NotPick it is `i == arr.size()`. In FOR loop approach base case isn't required since FOR loop range takes care of array traversal bounds

Whenever we require to remove duplicates (uniqueness), we `sort` and then use the FOR loop approach. Ex - ComboSum2, SubsetSum2. Sort condition: `if(i > p && arr[i] == arr[i - 1]) continue;`. We can further optimize the FOR loop using `if(arr[i] > target) break;` (if current element can't be part of the answer then remaining array can't be too; since array is sorted).
