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

- **TEMPLATE#1** - the "closed interval" binary search (`[low, high]`), the classic textbook binary search:
```cpp
int low = 0, high = arr.size() - 1;    // this makes it closed interval

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
int low = 0, high = arr.size();        // this makes it half-open interval

while(low < high){                    // line 1
  int mid = low + (high - low) / 2;

  if (k == arr[mid]) return mid;
  else if (k > arr[mid]) low = mid + 1;
  else high = mid;                    // line 2
}

// at this point in code, low = high (because of line 1)
// if method didn't return till now ofc

return -1;
```

**NOTICE**: `line 1` and `line 2` go hand-in-hand to make sure we move correctly. Whenever in doubt, dry run the case `[1 2 3]` with `k = 1`. Half-open interval maybe counter-intuitive at times, so just avoid it completely!

## Bounds

**Lower Bound**: lower bound of `x` is the smallest index `i` such that `arr[i] >= x`. Ex - in `[2 4 5]`, lower bound of `3` is `4` (not `2`) and lower bound of `4` is `4` itself. Also, observe that LB of `2` in `[1,2,2,3]` is `2` at index `1`.

**Upper Bound**: upper bound of `x` is the smallest index `i` such that `arr[i] > x`.  Ex - in `[2 4 5]`, upper bound of `3` is `4` and upper bound of `4` is `5`. Also, observe that UB of `2` in `[1,2,2,3]` is `3` at index `3`.

Note that `LB(x) = UB(x)` if element `x` is not present in the array. Also, there maybe no LB/UB (`return -1`) if we go out of bounds searching for it.

One interesting observation for LB/UB is that when we break out of the `while` loop, the lower or upper bound is at index `low`, no matter whichever template we're using! ([clarification](https://chatgpt.com/share/691b20c3-c714-800e-832a-6ab86759d358))
- keep `arr[mid] = k` condition on the direction we want to move in to skip duplicates (obvious). In LB we move leftwards in duplicates, in UB we move rightwards in duplicates. At the end, `low` will always end up at the answer.
- if the LB or UB for a given `k` does not exist in the array, then after the loop `low == arr.size()`.

[Code](https://leetcode.com/discuss/study-guide/1675643/lower-bound-and-upper-bound)

Related Problems:
- https://leetcode.com/problems/find-smallest-letter-greater-than-target
- **Search Insert Position**: ([link](https://leetcode.com/problems/search-insert-position)) insert position is LB/UB only, depends on where question wants us to insert if element is already present in array.
- Problems on **counting occurrences of an element**: [link](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array), [link](https://leetcode.com/problems/element-appearing-more-than-25-in-sorted-array)

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
  - another way but linear TC: find any occurrence of it using BS (if not found return `-1`), either check `idx == -1` that means occurrence is `0`.If a valid `idx`, linearly scan its left half and right half for more occurrences (TC = `O(logn) + O(n) => O(n)`)

## 1D Arrays

Observations for sorted and then rotated arrays e.g. `[4,5,1,2,3]`:
- there will be a single inflection point (aka pivot) e.g. `5` in the above example
- elements leftwards of the pivot will be in desc order and to the rightwards will be in asc order
- in the given array (sorted and then rotated i.e. unsorted), if we calc a mid and check edges of halves w.r.t it, then only one half can be sorted and the other has to be unsorted; we can check which is which and move accordingly in problems below. This property holds in all further smaller unsorted halves too.
- duplicates can mess up edges checking so we need to handle that case separately (if duplicates maybe present)

**Search in rotated sorted array (no duplicates)**: ([link](https://leetcode.com/problems/search-in-rotated-sorted-array)) we can't tell which half to goto only by looking at `arr[mid]` and `target` here, we can only do that in sorted arrays (or sorted half). One half will always be sorted in a rotated array! check which one and goto it only if target is in its range, otherwise goto the other half (even if unsorted) i.e. iteratively looking for a sorted half.

**Search in rotated sorted array (duplicates present)**: ([link](https://leetcode.com/problems/search-in-rotated-sorted-array-ii)) same as above but with an additional condition where `arr[mid] == arr[low] && arr[mid] == arr[high]` causing indicision on which side to move to. If this condition is true, then do `low++; high--; continue;`, otherwise proceed just as in the previous problem.

### Convergence Search
**TEMPLATE#3** - searching on a space (convergence search) as opposed to a value search (for `k`):
```cpp
int low = 0, high = n - 1;    // closed interval
while(low < high){
  int mid = low + (high - low) / 2;
  if (condition)
    low = mid + 1;
  else
    high = mid;
}

// this may look like half-open interval template because of the loop condition and high = mid update, but its closed only because high = n - 1
// remember, we are converging to an element by shrinking the search space here unlike before where we skipped the element at mid by updating high = mid - 1
// also, this is the first-true / min-feasible goals template discussed below
```

> [!TIP]
> Stick to `while(low <= high)` for value search as it checks when `mid = low = high` too (i.e. single remaining element). Use `while(low < high)` for convergence search problems as no such value check is needed and `low` needs to satisfy convergence property at the end.

**Find minimum in rotated sorted array (no duplicates)**: ([link](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array)) leftmost element (`arr[low]` or `arr[mid]`) in the sorted half will be the lowest
- check which half is sorted, get minimum so far from it either `arr[low]` or `arr[mid]`, and go to the other part tracking global min at every step.
- a more terse way is compare `arr[mid] > arr[high]` to detect which side is sorted, discard the sorted side, and keep the unsorted side until `low` reaches the pivot. Avoid comparing `arr[low] <= arr[mid]` as it won't cover the case `[1,2,3,4,5]` where array is sorted but never rotated.
- **if duplicates are present** ([link](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii)): like in `[2,2,2,0,2]`, we need to add condition for `arr[mid] == arr[high]` then do `high--` to remove ambiguity on which side to goto. It doesn't harm the search space for min element as `arr[mid]` will still be present in space, we only removed its duplicate.

```cpp
int findMin(vector<int>& nums) {
    int low = 0, high = nums.size() - 1;    // notice

    while (low < high) {
        int mid = low + (high - low) / 2;

        if (nums[mid] > nums[high])      // important for min element finding
            low = mid + 1;
        else
            high = mid;                // looks half-open, but isn't
    }

    return nums[low];                // min is at low in the end
}

// in short - shrink until one element remains
```

**Find out how many times has an array been rotated**: can be found out from index of the min element.

**Check if array is sorted and rotated**: elements leftwards of the pivot will be in desc order and to the rightwards will be in asc order. After finding the pivot, check sorted property of left half and right half manually (TC = `O(n)`).

**Single element in a sorted array** ([link](https://leetcode.com/problems/single-element-in-a-sorted-array)): first occurrence is supposed to be at even index and other at odd, but after the single element, it will be vice-versa. Goto `mid` and if `mid % 2 == 0` check `mid+1`, if `mid % 2 != 0` check `mid-1`. Go in the direction of first single element everytime shrinking search space. Edge case is when array has only 1 element, handled implicitly with condition `while(low < high)`.

**Find peak element** ([link](https://leetcode.com/problems/find-peak-element)): calc `mid`, if its a corner (`arr[0]` or `arr[n-1]`) then peak is that corner value itself, return it. Else check peaks among `arr[mid-1]`, `arr[mid]` and `arr[mid+1]`, return if its a peak, else keep moving in the direction of the greater element (as it guarantees at least one peak - either the greater element or an extreme corner eventually). Edge case is when there is just a single element in the array, which is implicitly handled by loop condition `while(low < high)` and then return `low` at the end.

## BS on Space
Tips to identify when BS is an appropriate solution for searching on an answer space:
- asked to minimize/maximize some feasible value
- the feasible value has a fixed but large answer space
- feasibility behaves monotonically (`true true false false false`)
- feasibility check for a given guess can replace binary search comparison conditions to navigate in answer space

### Universal Templates 
Min-feasible is exactly equal to LB so both templates work normally for it, wheareas max-feasible is not UB but one index leftwards of UB so we tweak some things. Ex - `[1 2 2 2 3]`.

**FIRST-TRUE / MIN-FEASIBLE**: return lowest index where check(mid) becomes true. This is nothing but Lower Bound.
```py
# low < high version
low = 0, high = n - 1
while (low < high):
  mid = (low + high) / 2
  if (check(mid)):
    high = mid 
  else:
    low = mid + 1
return low

# low <= high version
low = 0, high = n - 1
while (low <= high):
  mid = (low + high) / 2
    if (check(mid)):
      high = mid - 1
    else:
      low = mid + 1
return low
```
> **Uses**: find exact target / pivot / unique element / minimum element, koko eating bananas, etc.

**LAST-TRUE / MAX-FEASIBLE**: return highest index where check(mid) is true. This isn't Upper Bound but an index lower than it.
```py
# low < high version
low = 0, high = n - 1
while (low < high):
  mid = (low + high + 1) / 2    # upper mid
  if (check(mid)):
    low = mid        # notice
  else:
    high = mid - 1
return low

# low <= high version
low = 0, high = n - 1
while (low <= high):
  mid = (low + high) / 2
  if (check(mid)):
    low = mid + 1
  else:
    high = mid - 1
return high          # notice; one index leftwards of UB (which will be at low)
```

**NOTE**`: mid` calc is for upper mid because we will be go in infinite loop in cases like `[2, 5]` as we're updating `low = mid`. So we need to calc mid in the direction we're moving in i.e. rightwards in this case because we want to find last-true or max-feasible.

> **Uses**: floor(sqrt(x)), max ribbon length, max feasible speed / capacity / threshold, aggressive cows, etc.

**Sqrt of a number (integer)**: ([link](https://leetcode.com/problems/sqrtx)) This is basically find max number `n` which satisfies `n*n <= x` i.e. `floor(sqrt(x))`. We can init `low = 1, high = x/2`, but then we'll need to handle smaller values `if(x < 2) return x`.
```cpp
int low = 0, high = x;
while (low <= high) {
  int mid = low + (high - low) / 2;
  if (mid * mid <= x)
    low = mid + 1;
  else
    high = mid - 1;
}
return high;
```

- for double precision: use an epsilon because `low` may never be truly equal to `high`, and updates are such that because we don't know how big a jump to take here (we take `1` in integer sqrt) since its a continuous space search.
```cpp
double eps = 1e-5;   // upto 5 digits after decimal
while( (high - low) > eps ){
  double mid = (low + high) / 2.0;
  // set low = mid
  // or high = mid
}
return low;
```

Similar problem: **Nth Root of a Number** using Binary Search

### Simulation
They are simple, just use convergence search templates to find minimum or maximum feasible in answer space:
- [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)
- [Minimum Number of Days to Make m Bouquets](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/)
- [Find the Smallest Divisor Given a Threshold](https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/): this is exactly Koko Eating Bananas just diff problem statement
- [Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/)

**Kth Missing Positive Number**: find out `no. of elements missing till current element = arr[i]-(i+1)`, answer will always be `no. of elements present that are strictly less than arr[i] + k` i.e. `i + k` when `arr[i]-(i+1) >= k` is satisfied for the first time
  - Shifting k `O(n)` solution - really smart one liner!
  - In BS solution we search on that `arr[i]-(i+1)` space and check it on every `mid` and move accordingly, on `==` condition we have exactly `k` missing elements in left of `mid` and our ans lies just below `arr[mid]` (`ans = i+k`), we want LB (smallest index such that `arr[i]-(i+1) >= k`) so move leftwards!, then return `low + k` after loop break

**Split Array into K Subarrays**: aka painter's partition, book allocation, capacity to ship packages
- `low = max_element_of_array` and `high = sum_of_all_elements_of_array` and keep searching for lower value that can accomodate `k` max partitions (simulate) for each `mid`
- on equal condition, move leftwards to minimize max sum, return `low` at the end

## 2 Sorted Arrays
**Find Kth element of two sorted arrays**: 
  - count approach; mimic merge and count till K
  - cutpoints approach, `k = cut1 + cut2`, take `cut1` elements from one and `cut2` from the other [link](https://takeuforward.org/data-structure/k-th-element-of-two-sorted-arrays)
    - do BS on the smaller array only, compare `l1 r2 l2 r1`

**Median of two sorted arrays**: use cutpoints approach above `left_half = (m + n + 1) / 2`, take care of cuts min/max, and median calc condition(s) in case total elements are even or odd [link](https://takeuforward.org/data-structure/median-of-two-sorted-arrays-of-different-sizes/)

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











































