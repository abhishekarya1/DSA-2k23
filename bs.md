```cpp
mid = floor(low+high)/2;

mid = floor(low+(high-low))/2;   // avoids overflow
```

- Two ways to goto other half:
  - add or subtract `1` to `mid`
  - set `mid` to `high`/`low`

- Two ways to check value at mid:
  - when you know what you're looking for: check `mid` and skip it when moving to the other half
  - when we aren't looking for a specific value (ex - `min`): set `min` to `arr[low]` and move to the other half  
  
---
- Upper Bound/Lower Bound: keep moving towards right/left
- Search Insert Position: insert position will be lower bound only
- Check if array is sorted and rotated: use the property that both halves of the array are sorted (dividing point is the pivot, pivot = largest element), keep going to the sorted array if it lies in its range, else goto the other half, after finding pivot, check left half and right half manually for sort property (Time = `O(n)`)
- Find the first or last occurrence of a given number in a sorted array: keep moving left and find leftmost occurance, then reset and keep moving right and find rightmost occurance
- Count occurrences of a number in a sorted array with duplicates: Do same as above and count will be `rightmost_index - leftmost_index`
- Find peak element: Keep moving in the direction of the greater element and if we reach a corner (`arr[0]` or `arr[n-1]`) then peak is that corner value
- Search in rotated sorted array (no duplicates) - goto the sorted half if in range, else goto the other half
- Search in rotated sorted array (duplicates present) - check condition `arr[mid] == arr[0] && arr[mid] == arr[n-1]` and if true do `low++; high--;`, rest is the same as above
- Find minimum in Rotated Sorted Array: leftmost elementin the sorted half will be the lowest, keep going to sorted halves
- Find out how many times has an array been rotated: answer will be the index of the minimum element
- Single element in a Sorted Array: goto `mid` and check `mid-1`
- Find Kth element of two sorted arrays: cutpoints approach
