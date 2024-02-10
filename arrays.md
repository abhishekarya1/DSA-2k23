- Union of two sorted arrays: always take minimum of the two and check with equality with the last inserted element so that we don't insert duplicates `O(m+n)`

- Find one missing and one repeated numbers in an array:
  - Hashtable approach
  - `S = {1 + ... + n}` and `P = {1^2 + ... + n^2}` approach: form two equations and solve, overflow issue may happen tho 
  - XOR approach - `n & ~(n-1)` to keep only the rightmost set bit, `n` is XOR of whole array, form two buckets that have `0` and `1` at that position from `1 ... n` and given array
  - mark with `-1` approach - optimal linear TC

- Kadane's Algorithm (subarray having max sum): make sure to take `maxSum = INT_MIN` and not `0` since if all elements are negative in the array, it will print max sum as `0` incorrectly
	- Modify kadane's algo to keep track of start and end of max sum subarray
   	- brute force (cubic) approach, better (qudratic) approach, kadane's algo (linear)

- Print max sum contiguous subarray: Modified Kadane's algorithm - update `start_index = i + 1` on negative sum case, on new maxSum case update `end_index = i`, and print start and end, and return

- Rearrange alternate positve and negatives:
  - if stability is not required, segregate and place in `O(n)` time and `O(1)` space using swaps
  - if stability is required and positive and negative elements are equal: place in `res` array by flling even and odd indexes
  - if stability is required and positive and negative elements are not equal: use two arrays to place positive and negatives, put elements alternatingly in the original array, and after smaller aray is copied fully put remaining of the other array

- Sort an array of 0s, 1s, and 2s (Dutch-Flag Algorithm): take 3 pointers `lo=0` `mid=0` `hi=n-1`, `mid` is our "main" pointer; while `mid <= hi` do
	- on `0` swap `arr[lo]` and `arr[mid]`, increment both (sending `0` to `lo`, it is guranteed that element coming from `lo` will be `1`)
	- on `1` increment `mid` (not touching `1`)
	- on `2` swap `arr[mid]` and `arr[hi]`, only decrement `hi` (bcoz no guarantee that element coming from `arr[hi]` isn't `2`) (sending `2` to `hi`)
   
**NOTE**: Two pointer approach with `low` and `high` had condition `while(low < high)` but Dutch-National Algo has `while(mid <= high)`, why the `<=`? Because value of the last element of the separation (`low == high`) won't matter, it can be either of `0` or `1` and separation will still be valid. But with three-partitions, it can be `0` in cases like `[1, 0, 2]` (`mid = high = 1`) and we do need to process `0` and place it appropriately.
- Stock Buy and Sell:
  - Buy on one day, sell on another (single transaction): Maintain minimum so far (local minima), calculate profit on each day, and track maxProfit
  - Stock Buy and Sell-II: in this we can buy and sell multiple times a week but only one at a time. Use valley-peak approach: calc and add to profit on valley to peak but skip on peak to valley (as its a loss).

- Next Permutation: find first COUNTER-INVERSION from right, consider element on the left (`i`), find first number from right greater than it, swap them, reverse from `i+1` till the end. Edge case is when no counter-inversion is found, this means array is reverse sorted `3 2 1` and next permutation is reverse sort of it `1 2 3`
	- **LOGIC**: split will have smaller elements on left side and greater on right side (find counter-inversion), bring smallest from right half to left side (find greater and swap), need smallest possible permutation as next so we sort (reverse)

- Longest subarray with given sum K - generate all subarrays (3 loops), using 2 loops, maintain a prefix sum map `prefixSum[sum] = i` approach (hashing), two pointer approach (only this approach won't work if negatives are present in the array), if negatives aren't present optimal approach will be a sliding window
- Count subarrays with given sum K (or xor K): brute force cubic approach, better qudratic approach, `prefixSum[sum] = count` map approach (optimal)
- Longest consecutive sequence in an array: array can be unsorted
	- `O(n^3)` approach with a loop and a while loop to iterate till we keep finding `element + 1` for each element, find using linear search
	- `Onlogn` sort and do pairwise adjacent comparison and calc run length max for consecutives
 	- use a populated `set` and do linear traversal on it, if `element - 1` is not present in set, this is where we start run length and set `cnt = 1` and keep checking set for `element + 1` and updating `cnt++`, if `element-1` is present it would've been counted by previous run length so do nothing

- Longest subarray with 0 sum: same as above, but we don't see the same `sum` ever again (if we see it, we calculate length, not store it back) so we need not check existance before storing in `prefixSum` map unlike above approach, remember to initialize `mp[0] = 1` (sum 0 seen 1 time even before array traversal starts)

- Boyer-Moore Majority Voting Algorithm: Find majority element occuring `n/k` times
	- only one element can occur more than `n/2` times, max two elements can occur more than `n/3` times each
	- if `count == 0` set `maj_element = curr_element` and `count = 1`, else if `maj_element == curr_element` increment count, else decrement
	- scan again to verify majority status
	- incase of two elements (`n/3`), check numbers before count zero condition, also use `else if` to avoid updation of both `count1` and `count2` simultaneously on `count = 0`, else in decrement case, decrement both
- 3-Sum Problem: fix `i` pointer and do 2-pointer search for `target-arr[i]` in the rest of the array. Do this for all elements. 
	- incase duplicates are there, only consider first/last among the chain
	- time complexity: `O(n^2)`
- 4-Sum Problem:
	- strategy-1: fix two pointers `i = 0` and `j = n - 1` and search for `target-arr[i]` in the rest of the array (till `j > i`) using another two pointers, nested loops to inc `i` and dec `j`
	- strategy-2: fix two pointers `i = 0` and `j = i + 1` and search for `target-arr[i]` in the rest of the array using another two pointers, nested loops to inc `i` and inc `j`
  	- incase duplicates are there, only consider first/last among the chain
	- time complexity: `O(n^3)`
- Merge two sorted arrays in O(1) space:
	- Insertion sort approach: traverse larger array and swap the smaller one from then other array, resort the smaller array after every swap
	- Shell sort (Gap) approach: initiate `gap=(m+n)/2` and keep swapping inversions on gap pointers, reduce `gap/=2` every traversal of both arrays (`m+n` length), stop on `gap=0`
- Count Inversions / Reverse Pairs - linear solution is applicable to sorted array, but we can do mergeSort (`O(nlogn)`) and during/before merging count pairs for all small sub-partitions and total them and return at the end

### 2-D Matrix
- Search an element in 2D matrix: start from top-right or bottom-left corner
- Rotate Matrix by 90 degrees: transpose, and swap cols (aka reverse all rows)
- Set Matrix Zeros: use variable `col` as indicator for `col0`, start biulding reference matrix from `0, 0` and start building answer matrix from `arr[m-1][n-1]`, treat `col0` separately, both during building reference and answer matrix
- Spiral Traversal of Matrix: use 4 `for` loops bounded by 4 pointers (`left`, `right`, `down`, `up`), update after every `for` loop, do this while `up <= down && left <= right`, take care of edge case where there is only 1 row or 1 column. Pointers have to be placed very strategically (see [this](https://takeuforward.org/data-structure/spiral-traversal-of-matrix/) for a diagram).
- primary diag = `mat[i][i]`, sec diag = `mat[i][n - 1 - i]` (square matrix of `n x n` dimensions)
- convert 1D array into 2D matrix - `mat[i / rowSize][i % rowSize] = arr[i]` [problem](https://leetcode.com/problems/convert-1d-array-into-2d-array/) (useful in binary search, matrix problems, etc)

--- 

### Two ways of two-pointer
https://leetcode.com/articles/two-pointer-technique (segregating of elements into two halves can be done with both!) - `low` and `high` method is not STABLE though!

- ahead and behind pointers .aka. one pointer always moving trick [STABLE]
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
- low and high pointer trick [NOT STABLE]
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

Related Questions:
- https://leetcode.com/problems/remove-duplicates-from-sorted-array/
- Segragate 0s and 1s
- https://leetcode.com/problems/move-zeroes/ (LC sol requires stable way)

### Subarray problems 
Can be solved in following ways:
- generate all subarrays (cubic)
- calc subarray starting at `i` and ending at each position `j` with two loops (qudratic)
- solution involving space like `prefixSum` map (optimal)

### Duplicate/Missing Detection Techniques
```
- brute force (quadratic solution)
- freq count hashmap
- if only one is missing and/or only one is duplicate, use Math or XOR (pairings must exists for XOR, not applicable in more than 2 missing and/or more than 2 duplicates)
- sort and compare adjacents (works most time xD)
- mark with negatives and cycle sort approach
- binary search (on [1-n] search space)
- floyd's cycle detection algorithm (treat index and addresses and array elements as pointers)
```

**Find the Duplicate Number**: here the single duplicate number can occur more than 2 times, thats why XOR/Math isn't applicable, other ways are but modifications to input array and extra space isn't allowed [problem](https://leetcode.com/problems/find-the-duplicate-number/)
- binary search (no sorting required): search on `[1 - n]` space and simulate counting (`if(nums[i] <= mid) cnt++`) for each `mid`, TC = `O(nlogn)` even without sorting [OPTIMAL]
- floyd's cycle detection algorithm (Hare and Tortoise Two Pointers): elements of the array can be considered as pointers that points to the address (index) of next element, cycle is guaranteed because of pigeonhole principal and we can apply LL cycle starting point logic here, TC = `O(n)` [OPTIMAL] [video](https://www.youtube.com/watch?v=wjYnzkAhcNk)

[reference to all approaches above](https://leetcode.com/problems/find-the-duplicate-number/solutions/1892921/9-approaches-count-hash-in-place-marked-sort-binary-search-bit-mask-fast-slow-pointers/)

**First Missing Positive**: [video](https://www.youtube.com/watch?v=-lfHWWMmXXM)
- `O(n logn)` sorting approach
- `O(n)` time, `O(n)` space hashmap approach
- `O(n)` time, `O(1)` space approaches
	- multiply with `-1` (works only with +ve elements)
 	- cycle sort swapping (simulate 1-based indexing for ease)

**Find All Numbers Disappeared in an Array**: [link](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)
- brute-force with linear search (quadratic)
- sort and brute-force but use binary search (nlogn + nlogn)
- typical hashmap approach
- optimal - mark with negative approach; same as above ques
- optimal - swap approach; same as above ques

**Contains Duplicate at atmost distance k**: [link](https://leetcode.com/problems/contains-duplicate-ii/)
- optimal - track last index in `hash[]` and if its the second time we're seeing the element, calc delta and compare with `k` (linear SC)

**Find All Duplicates in an Array**: since multiple are missing, and multiple are repeating, we can't use sum or xor here. [link](https://leetcode.com/problems/find-all-duplicates-in-an-array/solutions/4595812/o-n-time-o-1-space-optimal-approach-using-marking-negative-technique-c/)
- optimal - marking with `-1` approach (use `abs()` and 1-based indexing) - if element is already marked, then its a duplicate

PATTERN: If there are multiple elements repeating and missing, sum or xor may not be applicable. We can use indices as hashmap! Since numbers are `[1 - n]` and indices are `[0 - n-1]`, convert to 1-based indexing and if only positives are there use negative marking technique, otherwise use swap technique.

### Two Pointers - Valleys

**Count Hills and Valleys in an Array**: [link](https://leetcode.com/problems/count-hills-and-valleys-in-an-array)
- instead of looking rightwards for next non-equal element to check peak/valley, keep track of leftwards non-equal element in a variable (`left`)
- compare next elements with this leftwards variable (`left`) instead of element `arr[i - 1]` to check peak/valley
- the valley/peak count will still remain same since we'll count only once on either side of the equal element occurance chain (smart)

**Trapping Rainwater Problem**: see [stack and queues notes](/stack_queue.md)

### Frequency Based
**Unique Number of Occurrences** - create freq map and put all freq in a set and compare set size with map size, another approach can be to count freq and sort freq array to identify if any duplicates are there by comparing adjacent elements. [problem](https://leetcode.com/problems/unique-number-of-occurrences/)

**Count Elements with Maximum Frequency** - multiple scans, create freq map and find max freq in it, then scan string and identify what all elements have the max freq (= cnt), ans will be `cnt * maxFreq`. [problem](https://leetcode.com/problems/count-elements-with-maximum-frequency/)

**Sort Characters by Frequency** (VERY IMPORTANT) - bucket sorting, create freq map of all characters and then append characters freq times to the corresponsing bucket in the `buckets` array of size `str.size() + 1` since max no. of times an element can ooccur is `s.size()`, form `ans` string by traversing and appending all non-empty buckets from the back. [problem](https://leetcode.com/problems/sort-characters-by-frequency/)
- since there is no way to sort freq numbers in map, we have an already sorted space (buckets) where we can attach elements and traverse from its back to get desc order of freq

---

**Product of Array Except Self**:
- calc entire array `prod` and div by `arr[i]` each time (linear time but overflow; div may not be allowed)
- for each element calc entire array `prod` skipping `arr[i]` in their respective iteration (quadratic time)
- track `prefixProd` and `suffixProd` (linear time, linear space)
- directly multiply preProd (first scan) and suffProd (second scan) to `ans` array in previous approach saving space (linear time, constant space)
