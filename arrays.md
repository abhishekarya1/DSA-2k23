- Find one missing and one repeated numbers in an array:
  - Hashtable approach
  - `S = {1 + ... + n}` and `P = {1^2 + ... + n^2}` approach: form two equations and solve
  - XOR approach - `n & ~(n-1)` to keep only the rightmost set bit, `n` is XOR of whole array, form two buckets that have `0` and `1` at that position from `1 ... n` and given array

- Kadane's Algorithm: make sure to take `maxSum = INT_MIN` and not `0` since if all elements are negative in the array, it will print max sum as `0` incorrectly
	- Modify kadane's algo to keep track of start and end of max sum subarray 

- Print Max continuous subarray: Modified Kadane's algorithm - update `start_index = i + 1` on negative sum case, on new maxSum case update `end_index = i`, and print start and end, and return

- Rearrange alternate positve and negatives:
  - if stability is not required, segregate and place in `O(n)` time and space
  - if stability is required and positive and negative elements are equal: place in `res` array by filling even and odd indexes
  - if stability is required and positive and negative elements are not equal: use two arrays to place positive and negatives, put elements alternatingly in the original array, and after smaller aray is copied fully put remaining of the other array

- Sort an array of 0s, 1s, and 2s (Dutch-Flag Algorithm): take 3 pointers `lo=0` `mid=0` `hi=n-1`, `mid` is our "main" pointer; while `mid <= hi` do
	- on `0` swap `arr[lo]` and `arr[mid]`, increment both (sending `0` to `lo`, it is guranteed that element coming from `lo` will be `1`)
	- on `1` increment `mid` (not touching `1`)
	- on `2` swap `arr[mid]` and `arr[hi]`, only decrement `hi` (bcoz no guarantee that element coming from `arr[hi]` isn't `2`) (sending `2` to `hi`)
- Stock Buy and Sell:
  - Maintain minimum so far (local minima), calculate profit on each day, and track maxProfit

- Next Permutation: find first COUNTER-INVERSION from right, consider element on the left (`i`), find first number from right greater than it, swap them, reverse from `i+1` till the end. Edge case is when no counter-inversion is found, this means array is reverse sorted `1 2 3` and next permutation is reverse sort of it `1 2 3`
- Boyer-Moore Majority Voting Algorithm: Find majority element occuring `n/k` times
	- only one element can occur more than `n/2` times, max two elements can occur more than `n/3` times each
	- if `count == 0` set `maj_element = curr_element` and `count = 1`, else if `maj_element == curr_element` increment count, else decrement
	- scan again to verify majority status
	- incase of two elements (`n/3`), check numbers before count zero condition, also use `else if` to avoid updation of both `count1` and `count2` simultaneously on `count = 0`, else in decrement case, decrement both
- 3-Sum Problem: fix `i` pointer and do 2-pointer search for `target-arr[i]` in the rest of the array. Do this for all elements. 
	- incase duplicates are there, only consider last among the chain (edge case)
	- time complexity: `O(n^2)`
- Longest subarray with give sum K - generate all subarrays (3 loops), using 2 loops, maintain a prefix sum map `prefixSum[sum] = i` approach (hashing), two pointer approach
- Longest subarray with 0 sum: same as above, but we don't see the same `sum` ever again (if we see it, we calculate length, not store it back) so we need not check existance before storing in `prefixSum` map unlike above approach, remember to initialize `mp[0] = 1` (sum 0 seen 1 time even before array traversal starts)
- Count subarray with given XOR K: same as count subarray with given sum k
- Merge two sorted arrays in O(1) space:
	- Insertion sort approach: traverse larger array and swap the smaller one from then other array, resort the smaller array after every swap
	- Shell sort (Gap) approach: initiate `gap=(m+n)/2` and keep swapping inversions on gap pointers, reduce `gap/=2` every traversal of both arrays (`m+n` length), stop on `gap=0`

### 2-D Matrix
- Search an element in 2D matrix: start from top-right or bottom-left corner
- Rotate Matrix by 90 degrees: transpose, and swap `col1` and `col2` (aka reverse all rows)
- Set Matrix Zeros: use `arr[0][0]` as indicator for `row1`, and variable `C` as indicator for `col1`, start building answer matrix from `arr[n-1][n-1]`, treat `col1` separately, both during building reference and answer matrix
- Spiral Traversal of Matrix: use 4 `for` loops bounded by 4 pointers (`left`, `right`, `down`, `up`), update after every `for` loop, do this while `up <= down && left <= right`

--- 

Two ways of two-pointer: https://leetcode.com/articles/two-pointer-technique (segregating of elements into two halves can be done with both!) - `low` and `high` method is not STABLE though!

- ahead and behind pointers .aka. one pointer always moving trick [STABLE]
```cpp
int i = 0, j = 0, n = arr.size();
while(i < n){
	if(arr[j] != arr[i]){	// increment j only when we want to insert
		j++;
		swap(arr[i], arr[j]);
	}
	
	i++;	// always increment i
}

return j+1;	// j points to 1 element rightwards to our ans list
```
- low and high pointer trick [NOT STABLE]
```cpp
int low = 0, high = nums.size() - 1;

while(low <= high){

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
