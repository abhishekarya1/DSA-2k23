## Intro
**Joke**: https://labuladong.gitbook.io/algo-en/iii.-algorithmic-thinking/detailedbinarysearch
> gotcha in the above joke is that Binary Search is only applicable when you're absolutely certain that the target element doesn't lie in the half being discarded. Works for sorted numbers, but not for a pile of books :D

## Templates
Binary Search impl can be extremely tricky because of checking and updating search space bounds. [Ref](https://stackoverflow.com/questions/504335/what-are-the-pitfalls-in-implementing-binary-search)

```cpp
mid = floor(low+high)/2;

mid = floor(low+(high-low))/2;   // avoids overflow
```

Two templates of writing BS (they differ by interval range `[]` vs `[)`):

- **TEMPLATE#1** - the "closed interval" binary search (`[low, high]`), the "classic" textbook binary search:
```cpp
int low = 0, high = arr.size() - 1;

while(low <= high){
  int mid = low + (high - low) / 2;

  if (k == arr[mid]) return mid;
  else if (k > arr[mid]) low = mid + 1;
  else high = mid - 1;
}

// at this point in code, low = high + 1
// if method didn't return till now ofc

return -1;
```

- **TEMPLATE#2** - the "half-open interval" binary search (`[low, high)`), C++ STL-like impl:
```cpp
int low = 0, high = arr.size();        // notice

while(low < high){                    // notice
  int mid = low + (high - low) / 2;

  if (k == arr[mid]) return mid;
  else if (k > arr[mid]) low = mid + 1;
  else high = mid;                    // notice
}

// at this point in code, low = high
// if method didn't return till now ofc

return -1;
```

### Bounds

**Lower Bound**: lower bound of `x` is the smallest index `i` such that `arr[i] >= x`. Ex - in `[2 4 5]`, lower bound of `3` is `4` (not `2`) and lower bound of `4` is `4` itself.

**Upper Bound**: upper bound of `x` is the smallest index `i` such that `arr[i] > x`.  Ex - in `[2 4 5]`, upper bound of `3` is `4` and upper bound of `4` is `5`.

Note that `LB(x) = UB(x)` if element `x` is not present in the array. Also, there maybe no LB/UB (`return -1`) if we go out of bounds searching for it.

One interesting observation for LB/UB is that when we break out of the `while` loop, the lower or upper bound is at index `low`, no matter whichever template we're using! ([clarification](https://chatgpt.com/share/6916154c-1de4-800e-bc95-9225b8772663))
- keep `arr[mid] = k` condition on the direction we want to move in to skip duplicates (obvious). In LB we move leftwards in duplicates, in UB we move rightwards in duplicates. At the end, `low` will always end up at the answer.
- if the LB or UB for a given `k` does not exist in the array, then after the loop `low == arr.size()`.

[Code](https://leetcode.com/discuss/study-guide/1675643/lower-bound-and-upper-bound)

Related Problems:
- https://leetcode.com/problems/find-smallest-letter-greater-than-target
- **Search Insert Position**: ([link](https://leetcode.com/problems/search-insert-position)) insert position is LB/UB only, depends on where question wants us to insert if element is already present in array.
- Problems on **counting occurances of an element**: [link](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array), [link](https://leetcode.com/problems/element-appearing-more-than-25-in-sorted-array)

> [!TIP]
> LB, UB, Floor, Ceil - Striver's way of storing in `ans` on every potential candidate is very simple and intuitive than above `low` pointer approach (TEMPLATE#2).

**Floor and Ceil** of `x`: if target is `4`, `low` and `high` will eventually converge at `[2 5]` and after two more steps, we'll have our **greatest number less than x** at `high` and **lowest number greater than x** at `low`. Take care of equal to cases and duplicates using strategy discussed above (for lower and upper bound)
- `ceil(x)` is equal to `LB(x)`, `floor(x)` can be the number smaller than `x` here (unlike `LB`) so its not at `low`, it's at `low - 1` (if `low > 0`) 
- if element `x` is present in array then `floor = x = ceil` (ofc)

**Find the first and last occurrence of a given number** in a sorted array:
  - First position: on `arr[mid] == k` store `ans = mid` and reduce search space to left array only (`high = mid - 1`), rest is same as in normal BS. Alternatively, first position is LB if number is present, check LB index to detect presence.
  - Last position: on `arr[mid] == k` store `ans = mid` and reduce search space to right array only (`low = mid + 1`), rest is same as in normal BS. Alternatively, first position is UB's index - 1 if number is present, check UB index to detect presence.

**Count occurrences of an element** in a sorted array: count will be `rightmost_index - leftmost_index + 1` 
  - use previous approach to find leftmost and righmost indices (with or without LB/UB)
  - another way but linear TC: find any occurance of it using BS (if not found return `-1`), either check `idx == -1` that means occurance is `0`.If a valid `idx`, linearly scan its left half and right half for more occurances (TC = `O(logn) + O(n) => O(n)`)

## BS on 1D Arrays
- Search in rotated sorted array (no duplicates) - we can't tell which half to goto only by looking at `arr[mid]` and `target` now, we can only do that in sorted arrays (or sorted half). Check which half is sorted - goto it only if target is in its range, otherwise goto the other half (even if unsorted) - iteratively looking for sorted half
- Search in rotated sorted array (duplicates present) - if `arr[mid] == k` is true then we've found our element, otherwise check condition `arr[mid] == arr[low] && arr[mid] == arr[high]` and if true do `low++; high--; continue;`, rest is the same as above
- Find minimum in Rotated Sorted Array: leftmost element (`arr[low]` or `arr[mid]`) in the sorted half will be the lowest, keep going to sorted halves and get minimum of it, and go to the other part
- Find out how many times has an array been rotated: answer will be the index of the minimum or maximum element
- Check if array is sorted and rotated: use the property that both halves of the array are sorted (dividing point is the pivot, pivot = largest element), after finding the pivot, check sorted property of left half and right half manually (time = `O(n)`)
- Find peak element: check peaks among `arr[mid-1]`, `arr[mid]` and `arr[mid+1]`, keep moving in the direction of the greater element, if we reach a corner (`arr[0]` or `arr[n-1]`) then peak is that corner value itself. corner case is when there is just a single element in the array `[2]` or we converged to a single element eventually (which will be one of the peaks), in that case don't go inside loop `while(low < high)` and `return start;` at the end
- Single element in a Sorted Array: first occurance is supposed to be at even index and other at odd, but after the single element, it will be vice versa. Goto `mid` and if `mid % 2 == 0` check `mid+1`, if `mid % 2 != 0` check `mid-1`. Go in the direction of single element everytime. Handle edge case of array having only 1 element using while loop range `low < high`.
- Find Kth element of two sorted arrays: 
  - count approach; mimic merge and count till K
  - cutpoints approach, `k = cut1 + cut2`, take `cut1` elements from one and `cut2` from the other [link](https://takeuforward.org/data-structure/k-th-element-of-two-sorted-arrays)
    - do BS on the smaller array only, compare `l1 r2 l2 r1`
- Median of two sorted arrays: use cutpoints approach above `left_half = (m + n + 1) / 2`, take care of cuts min/max, and median calc condition(s) in case total elements are even or odd [link](https://takeuforward.org/data-structure/median-of-two-sorted-arrays-of-different-sizes/)

## BS on Space

- Root of any number (can be float) using BS: Time = `O(N * log(M x 10^d))`
```cpp
double eps = 1e-5;   // upto 5 digits after decimal

while( (high - low) > eps ){

  double mid = (low + high) / 2.0;
  
  // set low = mid
  // or high = mid
  
}

return low;
```
---

**Kth Missing Positive Number**: find out `no. of elements missing till current element = arr[i]-(i+1)`, answer will always be `no. of elements present that are strictly less than arr[i] + k` i.e. `i + k` when `arr[i]-(i+1) >= k` is satisfied for the first time
  - Shifting k `O(n)` solution - really smart one liner!
  - In BS solution we search on that `arr[i]-(i+1)` space and check it on every `mid` and move accordingly, on `==` condition we have exactly `k` missing elements in left of `mid` and our ans lies just below `arr[mid]` (`ans = i+k`), we want LB (smallest index such that `arr[i]-(i+1) >= k`) so move leftwards!, then return `low + k` after loop break

**Split Array into K Subarrays**: aka painter's partition, book allocation, capacity to ship packages
- `low = max_element_of_array` and `high = sum_of_all_elements_of_array` and keep searching for lower value that can accomodate `k` max partitions (simulate) for each `mid`
- on equal condition, move leftwards to minimize max sum, return `low` at the end

## BS on Matrix

**Search in Row and Column Wise Sorted Matrix**:
- staircase search in `O(m + n)` TC
- rowwise and columnwise matrix after flattening isn't a sorted 1D matrix so that won't work
- the min in such matrix is `mat[0][0]` and max is `mat[m-1][n-1]` so we can perform bs in that space and search elements in each row (`m * log max(matEle)`)

**Kth Element in Row and Column Wise Sorted Matrix** [problem](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)
- same approach as above but count elements in each row using UB, and to find "actual" mid that is present in matrix do LB on search space (`m * log max(matEle)`)
- LB since we're trying to find smallest element (`mid`) in search space such that it gives us `k` elements in the matrix and actually present in the matrix

**Median of Row-wise Sorted Matrix**: points to note - matrix is sorted only rowwise, not column wise, and `r*c` is given to be odd
- find min and max elements of matrix from `0`th column and `c-1`th column respectively - this is our median search space
- perform binary search on this median space (calc `mid`) and for each row too use binary search to find numbers `<= mid` (i.e. calc upper bound of `mid` for each row) - **Nested Binary Search**
- use property that for an element `mid` to be median it needs to have `r*c/2` elements to the left of itself. If number of elements aren't sufficient or are excess, move left or right accordingly
- on equality condition `countSmallEqual(mid) == r*c/2`, our ans is first element having `countSmallEqual(mid) > r*c/2` (upper bound), so we move rightwards, `low` will eventually converge to UB

## Not From Sheet
**Find the Duplicate Number**: this can be optimally solved using BS or with Floyd's cycle detection [2k23 notes link](/arrays.md#duplicatemissing-detection-techniques)











