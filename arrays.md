## General
**Majority element `> n/2` times** approaches:
- brute force quadratic
- frequency hashmap
- sorting: sort and check mid (index `n/2`) (need to verify if its actually a majEle)
- building majEle with bitwise: count bits at each position (use `&` mask) for all numbers in array and if its `> n/2`, set it in `ans` (use `|` or) and that'll be majEle at the end (`O(n * log C)` where C can be max 32-bits for ints)
- optimal: boyer-moore voting algo (need to verify if its actually a majEle)

**Boyer-Moore Majority Voting Algorithm**: Find majority element occuring `n/k` times
- only one element can occur more than `n/2` times, max two elements can occur more than `n/3` times each
- if `count == 0` set `maj_element = curr_element` and `count = 1`, else if `maj_element == curr_element` increment count, else decrement
- scan again to verify majority status
- incase of two elements (`n/3`): two zero conditions and they have to check if currEle is not the other majEle as well, use `else if` to avoid updation of both `count1` and `count2` simultaneously on `countN = 0`, else in decrement case, decrement both

**Single element occuring more than `n/4` times** (`25%`) [link](https://leetcode.com/problems/element-appearing-more-than-25-in-sorted-array) - sort array and check majority count (> n/4) for each quadrant border (n/4, n/2, 3n/4, n-1) using Binary Search (use UB and LB)

**Max Consecutive Ones**: ([link](https://leetcode.com/problems/max-consecutive-ones)) just keep counter inc for `1` and dec for others (`0` here), and track max on break points only (better than tracking on each step; needs edge case handling where sequence doesn't end with a breakage but the loop reaches the end of array). 

**Longest consecutive sequence in an array**: array can be unsorted
- `O(n^2)` approach with two nested loops to iterate and keep finding `element + 1` for each element, find using linear search
- `Onlogn` sort and do pairwise adjacent comparison and calc run length max for consecutives
- use a populated `set` and do linear traversal on it, if `element - 1` is not present in set, this is where we start run length and set `cnt = 1` and keep checking set for `element + 1` and updating `cnt++`, if `element-1` is present it would've been counted by previous run length so do nothing

**Next Permutation**: find first COUNTER-INVERSION from right, consider element on the left (`i`), find first number from right greater than it, swap them, reverse from `i+1` till the end. Edge case is when no counter-inversion is found, this means array is reverse sorted `3 2 1` and next permutation is reverse sort of it `1 2 3`
- **LOGIC**: split will have smaller elements on left side and greater on right side (find counter-inversion), bring smallest from right half to left side (find greater and swap), need smallest possible permutation as next so we sort (reverse)

## Two Pointers
### Rotation and Reversal
[Reversal Algorithm](https://en.wikipedia.org/wiki/Block_swap_algorithms) to implement rotation

### Two ways of two-pointer
https://leetcode.com/articles/two-pointer-technique (segregating of elements into two halves can be done with both!) - `low` and `high` method is too unstable though!

- ahead and behind pointers .aka. one pointer always moving (_probe_) trick [MOSTLY STABLE]. Its not exactly stable but close! Example `{3,1,-2,-5,2,-4}` will get segregated as `{3,1,2,-5,-2,-4}` but a perfect segregation will be `{3,1,2,-2,-5,-4}`
```cpp
int i = 0, j = 0, n = arr.size();
while(i < n){
	if(arr[i] != 0){	// moving 0s to end
		swap(arr[i], arr[j]);
		j++;		// increment j only on insert
	}
	
	i++;	// always increment i
}
```
- low and high pointer trick

```cpp
int low = 0, high = nums.size() - 1;

while(low < high){

        // we need to move 0 to high
        if(nums[low] == 0){
        	swap(nums[low], nums[high]);
        	high--;
	}

        // else just move ahead, we don't have a 0 at low
        else
        	low++;
}
```

Related Problems:
- https://leetcode.com/problems/remove-duplicates-from-sorted-array/
- Segragate 0s and 1s
- https://leetcode.com/problems/move-zeroes/ (LC sol requires stable way)

### Segregating / Rearranging
**Rearrange alternate positve and negatives**: [link](https://leetcode.com/problems/rearrange-array-elements-by-sign) this problem CAN'T be solved correctly in `O(n)` time and  `O(1)` space by segregation followed by rearrangements in the same array as previously believed by me! The `O(n)` time two-pointer stable algo isn't always truly stable (its nearly stable) (_see template below_)
- using two extra arrays: place positive and negatives in them, put elements back alternatingly in the original array, and after smaller array is copied fully put remaining of the other array (if positives and negatives aren't equal in number)
- using one extra array: traverse over each element in the original array, place positives and negatives in the `ans` array at `evenIndex` and `oddIndex`, update both by `+= 2`
- if preserving order isn't required and the number of positives and negatives are equal then segregate them and perform swap on the first half of array with the second half (mirror) indices 
- there is a third way using two-pointers that uses `O(1)` space which is good for interviews as it works ONLY on smaller test cases: [link](https://leetcode.com/problems/rearrange-array-elements-by-sign/submissions/1175412097/)

**Odd element at odd index, Even at even index**: use last way above for constant space solution [problem](https://leetcode.com/problems/sort-array-by-parity-ii)

**Sort an array of 0s, 1s, and 2s (Dutch Flag Algorithm)**: take 3 pointers `lo=0` `mid=0` `hi=n-1`, `mid` is our "main" pointer; while `mid <= hi` do
- on `0` swap `arr[lo]` and `arr[mid]`, increment both (sending `0` to `lo`, it is guranteed that element coming from `lo` will be `1`)
- on `1` increment `mid` (not touching `1`)
- on `2` swap `arr[mid]` and `arr[hi]`, only decrement `hi` (bcoz no guarantee that element coming from `arr[hi]` isn't `2`) (sending `2` to `hi`)
   
**NOTE**: Two pointer approach with `low` and `high` had condition `while(low < high)` but Dutch-National Algo has `while(mid <= high)`, why the `<=`? Because value of the last element of the separation (`low == high`) won't matter, it can be either of `0` or `1` and separation will still be valid. But with three-partitions, it can be `0` in cases like `[1, 0, 2]` (`mid = high = 1`) and we do need to process `0` and place it appropriately.

### Merging two arrays
**Merging of K arrays** can be done on sorted arrays in `O(m+n+...)` time and constant space with `K` pointers. Tip - always copy smaller element and move its pointer only.

**Set operations** (merge, union, intersection, set diff)
- Non-Sorted: use ordered `map` or ordered `set` to sort and remove duplicates, or unordered ones for constant time lookups for unsorted arrays
- Sorted: use two-pointer technique discussed below
   
Duplicates within an array are problematic for two-pointer approach. Check last element of result array at each write to detect and eliminate duplicates.

- **Union** of two sorted arrays (sorted; duplicates present): always take minimum of the two and check with equality with the last inserted element so that we don't insert duplicates `O(m+n)`

- **Intersection** of two arrays (unsorted; duplicates present): [link](https://leetcode.com/problems/intersection-of-two-arrays/)
	- sort and do two-pointer approach like the Union approach above, to avoid duplicates check the last inserted element in `ans` array with the element to be inserted (skip if same)
	- sort and remove duplicates from each array using sets early on and do simple two-pointer approach
	- sort and do two-pointer and remove duplicates from `ans` array using a set at the end
	- store all elments of `nums2` in a freq map (acts as a set since it keeps unique elements only as keys), then traverse `nums1` and erase when an element is found in both (is intersection) to avoid duplicates; no sorting was required here because of map lookups (finds)
- **Set difference** of two sorted arrays: similar code as above, take smaller element but only if its in `A` and skip both on equal elements. Copy remaining but only from the `A` if calculating `A - B` set diff.

**Merge two sorted arrays in O(1) space**:
- Merging arrays approach: using two pointers, traverse both arrays and swap the smaller element from the second array to the first array, this fixes one element in the first (larger) array at appropriate position, re-sort the smaller array after every swap; sorting is necessary since we don't know what kind of elements are coming from first array upon swap.
- Shell sort (Gap) approach: initiate `gap=(m+n)/2` and keep swapping inversions on gap pointers, reduce `gap/=2` every traversal of both arrays (`m+n` length), stop on `gap=0`

### Divide and Conquer (Merge Sort based)
**Count Inversions / Reverse Pairs** - quadratic naive solution, linear solution is applicable to sorted-and-rotated arrays (with no duplicates) only, optimal solution is to do mergeSort (`O(nlogn)`) and during/before merging count pairs for all small sub-partitions and total them from every recurive call and return cumulative count at the end. Counting inversions can be done with modified condition body in the merge method, but counting reversals needs separate logic since we can't modify merge condition itself (we need scanning for reversals unlike inversions).

### Finding a target (2-Sum)
- https://leetcode.com/problems/two-sum-ii-input-array-is-sorted

**2-Sum Problem**:
- naive quadratic solution: pick one element and brute force search its other half (_addend_), use nested loops. Interseting observation is that we don't need to look backwards in the inner loop since pairing for those are already covered when outer loop was at previous indices.
- for unsorted: optimal approach is to find `target - arr[i]` while storing elements in a map/set.
- if sorted: fix `i = 0, j = n - 1` and do 2-pointer search for `target`.

**3-Sum Problem**: fix `i` pointer and do 2-pointer search for `target-arr[i]` in the rightwards part of the array from `i+1` to `n-1`. Do this for all elements.
- low and high technique won't work as previously believed by me! naive solution is `O(n^3)` here and will work the same as 2-Sum's naive sol (no need to scan previous indices).
- incase duplicates are there, only consider first/last among the chain using `while` loop in `i`'s loop and also inside 2-pointers loop but only on `== target` condition.
- TC = `O(n^2)` (_optimal_); map approach isn't possible here.

Related Problems: 
- https://leetcode.com/problems/3sum-closest
- https://leetcode.com/problems/3sum-smaller

**4-Sum Problem**:
- fix two pointers `i = 0` and `j = i + 1` and search for `target-arr[i]` in the rest of the array using another two pointers, nested loops to inc `i` and inc `j` to handle duplicates since it considers only the first/last among the chain.
  - taking search space from `j` till end works since we don't need to look at previous indices as they're already covered when we go left to right (with `i`) (as explained in 2-Sum above). 
- TC: `O(n^3)` (_optimal_)

To summarize:
```txt
2Sum → 2-pointer
3Sum → fix one, use 2-pointer
4Sum → fix two, use 2-pointer
and so on...
```

### Misc
- https://leetcode.com/problems/remove-element
- https://leetcode.com/problems/squares-of-a-sorted-array
- https://leetcode.com/problems/is-subsequence
- https://leetcode.com/problems/container-with-most-water [VERY IMPORTANT]

## Missing / Single / Duplicates
**Techniques**:
```
- brute force (quadratic solution)
- hashmap - store and check freq count
- set - put all elements in a set and compare set size with array size to check existence of duplicate(s)
- sort and compare adjacents (works xD)
- if only one is missing and/or only one is duplicate, use Math (only applicable if range is fixed like `[1 - n]` or `[m - n]`)
- form XOR buckets based on a single bit in XOR result of all elements (some sort of pairings must exists for XOR to work) (it works even if elements aren't in a fixed range)
- mark with negatives and cycle sort approach (treating array indices as hashmap so its equivalent to the above dedicated hashmap approach)
- binary search (on [1-n] search space)
- floyd's cycle detection algorithm (treat index and addresses and array elements as pointers)
```

**Some Simple Problems**:
- **Single missing number in range `[0 - n]`** (_math_ or _xor_): https://leetcode.com/problems/missing-number
- **Single number appearing once and others exactly twice** (_xor_ or _sort_): https://leetcode.com/problems/single-number
- **Tell if any number appears atleast twice (Contains Duplicate)** (_set_): https://leetcode.com/problems/contains-duplicate
- **Find duplicate when all other numbers appear exactly once**: use map, or sort and compare adjacents
- **Find number which appears once and others can appear multiple times**: map can always be used, better way is to use _sort_ and compare both adjacents (`arr[i-1]`, `arr[i]`, and `arr[i+1`) for each `i` to check for duplicates. Handle edge cases - first/last element being the single one.

**Find one missing and one repeated numbers in an array**:
- using Map to store freq counts
- `S = {1 + ... + n}` and `P = {1^2 + ... + n^2}` approach: form two equations and solve, overflow issue may happen though
- XOR approach - `n & ~(n-1)` to keep only the rightmost set bit, `n` is XOR of whole array, form two buckets that have `0` and `1` at that position from `1 ... n` and given array
- mark with `-1` approach - optimal linear TC

**Find 2 numbers which appear once and others appear twice**: [link](https://leetcode.com/problems/single-number-iii/) we can't apply maths approach here as the input range is not `[1 - n]`. Hence, we apply XOR bit buckets technique here.

**Find the Duplicate Number**: here the single duplicate number can occur more than 2 times, thats why XOR/Math isn't applicable, other ways are but modifications to input array and extra space isn't allowed [problem](https://leetcode.com/problems/find-the-duplicate-number/)
- binary search (no sorting required): search on `[1 - n]` space and simulate counting (`if(nums[i] <= mid) cnt++`) for each `mid`, TC = `O(nlogn)` even without sorting [OPTIMAL]
- floyd's cycle detection algorithm (Hare and Tortoise Two Pointers): elements of the array can be considered as pointers that points to the address (index) of next element, cycle is guaranteed because of pigeonhole principle and we can apply LL cycle starting point logic here, TC = `O(n)` [OPTIMAL] [video](https://www.youtube.com/watch?v=wjYnzkAhcNk)

[reference to all approaches above](https://leetcode.com/problems/find-the-duplicate-number/solutions/1892921/9-approaches-count-hash-in-place-marked-sort-binary-search-bit-mask-fast-slow-pointers/)

**First Missing Positive**: this problem has negatives and larger numbers, thats why its tricky [video](https://www.youtube.com/watch?v=-lfHWWMmXXM)
- `O(n logn)` sorting approach [link](https://leetcode.com/problems/first-missing-positive/solutions/4426658/best-and-simple-c-approach-by-khubaib-ahmad/)
- `O(n)` time, `O(n)` space separate array as hashmap/unordered_map approach or use set (still needs `O(n)` space)
- `O(n)` time, `O(1)` space approaches (marking indices approach - must utilize 0-based indexing to fit all elements) since possible ans is in the range `[1 to n+1]`, thus we need index for `n` in the worst case (if all in range `1-n` are present and `n+1` is the missing one)
	- mark by multiplying with `-1` (works only if +ve elements are guaranteed)
	- cycle sort swapping (works all the time)

**Find All Numbers Disappeared in an Array**: [link](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)
- brute-force with linear search (quadratic)
- sort and brute-force but use binary search (nlogn + nlogn)
- typical hashmap approach
- optimal - mark with negative approach; same as above ques
- optimal - swap approach; same as above ques

**Find All Duplicates in an Array**: since multiple are missing, and multiple are repeating, we can't use sum or xor here. [link](https://leetcode.com/problems/find-all-duplicates-in-an-array/solutions/4595812/o-n-time-o-1-space-optimal-approach-using-marking-negative-technique-c/)
- optimal - marking with `-1` approach (use `abs()` and 1-based indexing) - if element is already marked, then its a duplicate

**PATTERN**: If there are multiple elements repeating and missing in a range, sum or xor may not be applicable. We can use indices as hashmap! Since numbers are `[1 - n]` and indices are `[0 - n-1]`, convert to 1-based indexing and if only positives are there use negative marking technique, otherwise use swap technique. 

If there in no range, then XOR buckets technique is the best approach most times if we have amalgamation of two numbers after doing XOR on whole array.

## Prefix Sum
**Longest subarray with 0 sum**: maintain a prefix sum map `prefixSum[sum] = i`, if we see a sum again, that means the subarray between previous occurance till current occurance is the `0` sum subarray (calculate its length and track max for it), remember to init `mp[0] = -1` (i.e. sum `0` is always seen once even before array traversal starts on index `-1`), alt we can add condition `if(sum == 0) lenSub = i+1` too if we don't want to init map with `mp[0] = -1`.

**Longest subarray with given sum K** - generate all subarrays (3 loops), using 2 loops, maintain a prefix sum map `prefixSum[sum] = i` approach (hashing), two-pointer fixed SW approach (this approach won't work if negatives are present in the array), if negatives aren't present it will be the optimal approach otherwise prefix sum approach is optimal.

**Tip**: In these problems, we can either keep map init `mp[0] = -1` (since at index `-1`, sum is `0`) or condition `if(sum == k) lenSub = i+1` just after calc current sum. Also, since we're finding "longest" subarray in above problems, don't update hashmap entry upon re-occurance of a prefix sum value.

**Count subarrays with given sum K** (or xor K): brute force cubic approach, better qudratic approach, `prefixSum[sum] = count` map approach (optimal)
- the count of sum `K` subarrays till the current element will be subarrays from all previously seen `target = sum - k` sums till now, the prefix sum array stores their counts
- init `mp[0] = 1` or alt `if(sum == k) cnt++` to include the subarray from `0` till current as well in count
- update `mp[sum]++` on every step to maintain counts for seen sums

**Longest subarray with equal number of 0s and 1s** [link](https://leetcode.com/problems/contiguous-array):
- SW won't work here since its required that sum is always increasing in order to apply that (since once window left pointer is moved, it can't go back). It doesn't work in presence of negatives in sum k subarray problem too
- use two FOR loop apprach and calc size using `j - i + 1` on each valid subarray, internal loop is from `j = i to n-1` as usual
- use prefixSum map approach - use `cnt` to track sum (occurances), init `lastSeen[0] = -1` is important here, length calc is `i - lastSeen[cnt]`

**Product of Array Except Self**:
- calc entire array `prod` and div by `arr[i]` each time (linear time but may cause numeric overflow; division operation may not be allowed)
- for each element calc entire array `prod` skipping `arr[i]` in their respective iteration (quadratic time)
- track `prefixProd` and `suffixProd` (linear time, linear space)
- directly multiply preProd (first scan) and suffProd (second scan) to `ans` array in previous approach saving space (linear time, constant space)

**Contains Duplicate at atmost distance K**: [link](https://leetcode.com/problems/contains-duplicate-ii/)
- optimal - track last index in `hash[]` and if its the second time we're seeing the element, calc delta and compare with `k` (linear SC)

Related Problems:
- https://leetcode.com/problems/range-sum-query-immutable

## Leaders
**Count Hills and Valleys in an Array**: [link](https://leetcode.com/problems/count-hills-and-valleys-in-an-array)
- instead of looking rightwards for next non-equal element to check peak/valley, keep track of leftwards non-equal element in a variable (`left`)
- compare next elements with this leftwards variable (`left`) instead of element `arr[i - 1]` to check peak/valley
- the valley/peak count will still remain same since we'll count only once on either side of the equal element occurance chain (smart)

**Stock Buy and Sell**:
- Buy on one day, sell on another (single transaction): Maintain minimum so far (local minima), calculate profit on each day, and track maxProfit
- Stock Buy and Sell-II: in this we can buy and sell multiple times a week but only one stock at a time. To maximize profit, sell on the peak that immediately follows a valley i.e. calc and add to profit on valley to peak but skip on peak to valley (as its a loss).
	- _Intuition_ - we can only keep one stock at a given time, so we sell immediately as soon as we see a profit to be able to buy stock for a future price that maybe even lower than what we bought it for previously. If the price keeps increasing `[10, 20, 30]` then profit will still be `20` no matter if we buy once-sell once or multiple times. But it makes a diff in `[10, 20, 15, 30]` where a profit of `25` can be made by selling multiple times as opposed to single time profit of `20`.
 
**Trapping Rainwater Problem**: see [stack and queues notes](/stack_queue.md)

## Intervals
**Merge Overlapping Intervals**: ([link](https://leetcode.com/problems/merge-intervals)) 
- use two pointers, one in a separate `ans` array and one in current (starting from index `1`) and merge into `ans.back()`
- in-place - use two pointer technique just like "remove duplicate from sorted array" problem (i.e. init `i = 0, j = 0`, probe with `i` and insertion at index `j + 1`). At the end ans is all elements in range `[0 = j]`.

**Insert Interval**: ([link](https://leetcode.com/problems/merge-intervals)) add all non-overlapping array intervals to `ans` array till `newInterval` overlaps with anyone, then keep widening the `newInterval` based on overlaps with subsequent array intervals, insert the widened `newInterval` and then rest of non-overlapping array intervals. Be careful while merging, in previous problem intervals were in a sorted array, but here we are merging a new interval from outside to array so we need to set `newInterval[start] = min(currInterval[start], newInterval[start])` as well, apart from the usual `newInterval[end] = max(currInterval[end], newInterval[end])`.

**Non-overlapping Intervals**: ([link](https://leetcode.com/problems/non-overlapping-intervals)) sort by end time, then count non-overlapping intervals using a leader `end` value which has the end value of the last non-overlapping interval. Subtract with `intervals.size() - cnt` to get ans. Brute force quadratic comparison of intervals won't really give correct answer here on large test cases! Greedy is the only way to solve.
- _What gives min number of intervals we need to remove?_ Max number of intervals we need to keep. Suppose if one interval overlaps and deletes 3 of them (by merging), then its better to delete that one instead of those 3 i.e. keep interval deletion minimal. ([ref](https://leetcode.com/problems/non-overlapping-intervals/solutions/91713/java-least-is-most/comments/96271/))
- _Why sort by end time?_ This strategy is the classic greedy approach with scheduling problems. It ensures that the number of non-overlapping intervals is max because by always picking the interval that ends earliest, we leave as much room as possible for the remaining intervals. Consider the case `[[2,3], [1,4], [3,4]]`, if we pick `[1,4]` first (if sorted normally) then we lose out on both the others.

**Interval List Intersections**: ([link](https://leetcode.com/problems/interval-list-intersections)) we're comparing two intervals here which aren't even sorted unlike the merge intervals problem. 6 possible cases, out of which 4 are overlapping ([diagram](https://github.com/Chanda-Abdul/Several-Coding-Patterns-for-Solving-Data-Structures-and-Algorithms-Problems-during-Interviews/blob/main/%E2%9C%85%20%20Pattern%2004%20%3A%20Merge%20Intervals.md#intervals-intersection-medium)). Use two pointers and check overlapping cases with 2 conditions and calc intersections with max and min and store them, move ahead from interval which finishes first.

To summarize, this are the general conditions to detect and merge overlaps (if `==` is also considered overlap):

![](https://i.imgur.com/sQaa1sN.png)

We don't need to calc `startMerged` using `min` in Merge Intervals problem, because intervals are already sorted such that `startA <= startB` is always true so only above overlap condition is possible and no need to calc `startMerged` as it will always be `startA`. This wasn't the case with Interval Instersections problem so we checked everything there.

## Subarrays
Can be solved in following ways:
- generate all subarrays (cubic)
- calc subarray starting at `i` and ending at each position `j` with two loops (qudratic)
- solution involving space like `prefixSum` map (optimal)

Detailed observations and templates in [SWTP Notes](/swtp.md)

**Kadane's Algorithm** (subarray having max sum): make sure to take `maxSum = INT_MIN` and not `0` since if all elements are negative in the array, it will print max sum as `0` incorrectly
- brute force (cubic) approach, better (qudratic) approach, kadane's algo (linear; optimal; DP)
- _Intuition_: we keep summing and if we get a negative sum, the effect it will have when we sum further is net negative (reduction) and it will hurt max sum chances so we make it zero (`currSum = 0`) and start afresh from next element to continue looking for max subarray sum. If array has all negatives, `currSum = 0` happens at all indices and we only get max negative number in the end due to max tracking at each step.

**Print max sum contiguous subarray**: modified Kadane's algorithm; update `startIndex = i + 1` on negative sum case, on new maxSum case update `endIndex = i`, the max sum subarray is in range `[startIndex, endIndex]`

**Maximum Product Subarray**: ([link](https://leetcode.com/problems/maximum-product-subarray)) there are two ways to get max product, unlike max sum. If we're going too negative, that can also give us a big product later apart from the usual way of considering only positive sum in Kadane's algo. So track both `posProd` and `negProd` and update them both by multiplying with current array element.

```cpp
temp = negProd;
negProd = min({negProd * nums[i], nums[i], posProd * nums[i]});
posProd = max({temp * nums[i], nums[i], posProd * nums[i]});
maxProd = max(maxProd, posProd);
```

## Hashing / Frequency Based
**Unique Number of Occurrences** - create freq map and put all freq in a set and compare set size with map size, another approach can be to count freq and sort freq array to identify if any duplicates are there by comparing adjacent elements. [problem](https://leetcode.com/problems/unique-number-of-occurrences/)

**Count Elements with Maximum Frequency** - multiple scans, create freq map and find max freq in it, then scan string and identify what all elements have the max freq (= cnt), ans will be `cnt * maxFreq`. [problem](https://leetcode.com/problems/count-elements-with-maximum-frequency/)

**Sort Characters by Frequency** (VERY IMPORTANT) - bucket sorting, create freq map of all characters and then append characters freq times to the corresponsing bucket in the `buckets` array of size `str.size() + 1` since max no. of times an element can ooccur is `s.size()`, form `ans` string by traversing and appending all non-empty buckets from the back. [problem](https://leetcode.com/problems/sort-characters-by-frequency/)
- since there is no way to sort freq numbers in map, we have an already sorted space (buckets) where we can attach elements and traverse from its back to get desc order of freq

## 2-D Matrix
**Search an element in 2D matrix**: start from top-right or bottom-left corner

**Rotate Matrix by 90 degrees**: transpose, and swap cols (aka reverse all rows)

**Set Matrix Zeroes**: use variable `col` as indicator for `col0`, start biulding reference matrix from `0, 0` and start building answer matrix from `arr[m-1][n-1]`, treat `col0` separately, both during building reference and answer matrix

**Spiral Traversal of Matrix**: use 4 `for` loops bounded by 4 pointers (`left`, `right`, `down`, `up`), update after every `for` loop, do this while `up <= down && left <= right`, take care of edge case where there is only 1 row or 1 column. Pointers have to be placed very strategically (see [this](https://takeuforward.org/data-structure/spiral-traversal-of-matrix/) for a diagram).

**Some Tricks**:
- primary diag = `mat[i][i]`, sec diag = `mat[i][n - 1 - i]` (square matrix of `n x n` dimensions)
- convert 1D array into 2D matrix - `mat[i / rowSize][i % rowSize] = arr[i]` [problem](https://leetcode.com/problems/convert-1d-array-into-2d-array/) (useful in binary search, matrix problems, etc)
