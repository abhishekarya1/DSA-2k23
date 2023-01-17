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
- Upper Bound/Lower Bound: keep moving towards right/left with `mid + 1` or `mid - 1`, pointer will always land at element, `low` (upper bound) or `low - 1` (lower bound)
- Search Insert Position: insert position will be upper bound only
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
