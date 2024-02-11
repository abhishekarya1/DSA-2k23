```cpp
mid = floor(low+high)/2;

mid = floor(low+(high-low))/2;   // avoids overflow
```

Two templates of writing BS (they output the exact same thing and differ by interval close `[]` vs `[)`):
```cpp
int low = 0, high = arr.size() - 1;

while(low <= high){
  int mid = low + (high - low) / 2;

  if(k == arr[mid]) return mid;
  else if(k > arr[mid]) low = mid + 1;
  else high = mid - 1;
}

return -1;
```

```cpp
int low = 0, high = arr.size();

while(low < high){
  int mid = low + (high - low) / 2;

  if(k == arr[mid]) return mid;
  else if(k > arr[mid]) low = mid + 1;
  else high = mid;
}

return -1;
```

**NOTICE**: after breaking out of the `while` loop, the `low` and `high` values differ, this is important to find lower and upper bounds since we return `low` there.

[Optional Reading](https://labuladong.gitbook.io/algo-en/iii.-algorithmic-thinking/detailedbinarysearch) on above two templates of BS.

- Two ways to check value at mid:
  - when you know what you're looking for: check `mid` and skip it when moving to the other half by add or subtract `1` to `mid (find k, lower_bound, upper_bound, BS on array)
  - when we aren't looking for a specific value:
    - set `low` to `mid` to move to one half (ex - finding float root)
    - `low` and `high` will converge to the same element eventually 
---
- Lower Bound: lower bound of `x` is the smallest index `i` such that `arr[i] >= x`. Ex - in `[2 4 5]`, lower bound of `3` is `4` (not `2`) and lower bound of `4` is `4` itself
- Upper Bound: upper bound of `x` is the smallest index `i` such that `arr[i] > x`.  Ex - in `[2 4 5]`, upper bound of `3` is `4` and upper bound of `4` is `5`

Note that Lower bound is equal to Upper bound if element `x` is not present in the array. Also, there maybe no LB/UB (`return -1`) if `low` pointer crosses array bounds

Keep `arr[mid] = k` condition on the direction we want to move in to skip duplicates. In lower bound we move leftwards in duplicates, in upper bound we move rightwards in duplicates. `low` will always end up at the answer (rightwards element before condition is broken).

[Problem](https://leetcode.com/problems/find-smallest-letter-greater-than-target/)
[Code Templates](https://leetcode.com/discuss/study-guide/1675643/lower-bound-and-upper-bound)

LB, UB, floor, ceil - Striver's strategy of storing in `ans` on every potential candidate is very simple and intuitive than above `low` pointer approach.

Ceil value is equal to the Lower Bound value, also if target element is present then `floor = element = ceil`

- Floor/Ceil of `x`: if target is `4`, `low` and `high` will eventually converge at `[2 5]` and after two more steps, we'll have our **greatest number less than x** at `high` and **lowest number greater than x** at `low`. Take care of equal to cases and duplicates using strategy discussed above (for lower and upper bound)
- Search Insert Position: insert position is lower/upper bound only, depends on where question wants us to insert if element is already present in array
- Find the first or last occurrence of a given number in a sorted array: CANNOT be solved using lower/upper bound or floor/ceil bcoz they guarantee a valid index as output and don't return `-1` if element is not found in array
  - Find first position: on `arr[mid] == k` store `ans = mid` and reduce search space to left array only (`high = mid - 1`), rest is same as in normal BS
  - Find last position: on `arr[mid] == k` store `ans = mid` and reduce search space to right array only (`low = mid + 1`), rest is same as in normal BS
- Count occurrences of a number in a sorted array with duplicates: count will be `rightmost_index - leftmost_index`
  - use previous approach to find leftmost and righmost indexes
  - another way but linear TC: find any occurance of it using BS (if not found return `-1`), either check `idx == -1` then occurance is `0`, or if a valid `idx` linearly scan its left half and right half for more occurances (time = `O(n)`)
---
- Search in rotated sorted array (no duplicates) - we can't tell which half to goto only by looking at `arr[mid]` and `target` now, we can only do that in sorted arrays (or sorted half). Check which half is sorted - goto it only if target is in its range, otherwise goto the other half (even if unsorted) - iteratively looking for sorted half
- Search in rotated sorted array (duplicates present) - if `arr[mid] == k` is true then we've found our element, otherwise check condition `arr[mid] == arr[low] && arr[mid] == arr[high]` and if true do `low++; high--; continue;`, rest is the same as above
--- 
- Find minimum in Rotated Sorted Array: leftmost element (`arr[low]` or `arr[mid]`) in the sorted half will be the lowest, keep going to sorted halves and get minimum of it, and go to the other part
- Find out how many times has an array been rotated: answer will be the index of the minimum or maximum element
- Check if array is sorted and rotated: use the property that both halves of the array are sorted (dividing point is the pivot, pivot = largest element), after finding the pivot, check sorted property of left half and right half manually (time = `O(n)`)

---

- Find peak element: check peaks among `arr[mid-1]`, `arr[mid]` and `arr[mid+1]`, keep moving in the direction of the greater element, if we reach a corner (`arr[0]` or `arr[n-1]`) then peak is that corner value itself. corner case is when there is just a single element in the array `[2]` or we converged to a single element eventually (which will be one of the peaks), in that case don't go inside loop `while(low < high)` and `return start;` at the end
- Single element in a Sorted Array: first occurance is supposed to be at even index and other at odd, but after the single element, it will be vice versa. Goto `mid` and if `mid % 2 == 0` check `mid+1`, if `mid % 2 != 0` check `mid-1`. Go in the direction of single element everytime. Handle edge case of array having only 1 element using while loop range `low < high`.
- Find Kth element of two sorted arrays: 
  - count approach; mimic merge and count till K
  - cutpoints approach, `k = cut1 + cut2`, take `cut1` elements from one and `cut2` from the other [link](https://takeuforward.org/data-structure/k-th-element-of-two-sorted-arrays)
    - do BS on the smaller array only, compare `l1 r2 l2 r1`
- Median of two sorted arrays: use cutpoints approach above `left_half = (m + n + 1) / 2`, take care of cuts min/max, and median calc condition(s) in case total elements are even or odd [link](https://takeuforward.org/data-structure/median-of-two-sorted-arrays-of-different-sizes/)

---

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

---

**Search in Row and Column Wise Sorted Matrix**:
- staircase search in `O(m + n)` TC
- traverse 2D matrix using flattening - for `i` in range `0` to `R * C - 1` (array size if storing), total is `R * C`, access an element `matrix[row][col]` using `mat[i / C][i % C]` (if stored/accessed row-wise)
- rowwise and columnwise matrix after flattening will be sorted so we can apply binary search - `O(log(m*n))`

**Median of Row-wise Sorted Matrix**: points to note - matrix is sorted only rowwise, not column wise, and `r*c` is given to be odd
- find min and max elements of matrix from `0`th column and `c-1`th column respectively - this is our median search space
- perform binary search on this median space (calc `mid`) and for each row too use binary search to find numbers `<= mid` (i.e. calc upper bound of `mid` for each row) - **Nested Binary Search**
- use property that for an element `mid` to be median it needs to have `r*c/2` elements to the left of itself. If number of elements aren't sufficient or are excess, move left or right accordingly
- on equality condition `countSmallEqual(mid) == r*c/2`, our ans is first element having `countSmallEqual(mid) > r*c/2` (upper bound), so we move rightwards, `low` will eventually converge to UB
