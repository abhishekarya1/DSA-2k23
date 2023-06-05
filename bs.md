```cpp
mid = floor(low+high)/2;

mid = floor(low+(high-low))/2;   // avoids overflow
```

- Two ways to goto other half:
  - add or subtract `1` to `mid`
  - set `mid` to `high`/`low`

- Two ways to check value at mid:
  - when you know what you're looking for: check `mid` and skip it when moving to the other half
  - when we aren't looking for a specific value:
    - set `low` to `mid` to move to one half (ex - finding root)
    - `low` and `high` will converge to the same element eventually 
---
- Lower Bound: lower bound of `x` is the smallest index `i` such that `arr[i] >= x`. Ex - in `[2 4 5]`, lower bound of `3` is `4` (not `2`) and lower bound of `4` is `4` itself
- Upper Bound: upper bound of `x` is the smallest index `i` such that `arr[i] > x`.  Ex - in `[2 4 5]`, upper bound of `3` is `4` and upper bound of `4` is `5`

Keep `arr[mid] = k` condition on the direction we want to move in to skip duplicates. In lower bound we move leftwards in duplicates, in upper bound we move rightwards in duplicates. `low` will always end up at the answer.

- Floor/Ceil of `x`: if target is `4`, `low` and `high` will eventually converge at `[2 5]` and after two more steps, we'll have our **greatest number less than x** at `high` and **lowest number greater than x** at `low`. Take care of equal to cases and duplicates using strategy discussed above (for lower and upper bound)
- Search Insert Position: insert position is upper bound only
- Check if array is sorted and rotated: use the property that both halves of the array are sorted (dividing point is the pivot, pivot = largest element), keep going to the sorted array if it lies in its range, else goto the other half, after finding pivot, check left half and right half manually for sort property (Time = `O(n)`)
- Find the first or last occurrence of a given number in a sorted array: keep moving left and find leftmost occurance, then reset and keep moving right and find rightmost occurance
- Count occurrences of a number in a sorted array with duplicates: Do same as above and count will be `rightmost_index - leftmost_index`
- Find peak element: Keep moving in the direction of the greater element and if we reach a corner (`arr[0]` or `arr[n-1]`) then peak is that corner value
- Search in rotated sorted array (no duplicates) - goto the sorted half if in range, else goto the other half
- Search in rotated sorted array (duplicates present) - check condition `arr[mid] == arr[0] && arr[mid] == arr[n-1]` and if true do `low++; high--;`, rest is the same as above
- Find minimum in Rotated Sorted Array: leftmost element (`arr[low]`) in the sorted half will be the lowest, keep going to sorted halves and get minimum, goto the other part
- Find out how many times has an array been rotated: answer will be the index of the minimum or maximum element
- Single element in a Sorted Array: goto `mid` and if `mid % 2 == 0` check `mid+1`, if `mid % 2 != 0` check `mid-1`. Both the stated pairs need to be same value, otherwise go leftwards if they aren't. Handle edge case of array having only 1 element .
- Find Kth element of two sorted arrays: 
  - count approach; mimic merge and count till K
  - cutpoints approach, `k = m + n`, take `m` elements from one and `n` from the other (https://takeuforward.org/data-structure/k-th-element-of-two-sorted-arrays)
    - do BS on the smaller array only, compare `l1 r2 l2 r1`
---
- Root of a number using BS: Time = `O(N * log(M x 10^d))`
```cpp
double eps = 1e-5;   // upto 5 digits after decimal

while( (high - low) > eps ){

  double mid = (low + high) / 2.0; 
  
  // set low = mid 
  // or high = mid
  
}
```
