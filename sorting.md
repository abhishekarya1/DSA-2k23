- Selection Sort (Greedy)
- Bubble Sort (+ adaptive optimization) - swap inversions, n-1 passes, each pass places largest element of that pass (Kth largest) to righmost position (end)
- Insertion Sort - start from second element dividing array into sorted half and unsorted half, shift elements towards left while finding correct position in sorted half for one element from unsorted half
- Merge Sort (Divide & Conquer) - always use tight bounds to avoid confusion while merging `while(i <= mid && j <= high)`, all sort method calls will be tight bound too `mergeSort(arr, 0, n-1)`
- Quick Sort (Divide & Conquer)

### Terminology
**stable** - order of elements is maintained
- stable: bubble, insertion, merge
- unstable: selection, quick, heap

**in-place** - `O(1)` space
- bubble, insertion, selection, quick
- not in-place: merge, counting
 
**adaptive** - if list is sorted, sort process must not proceed. Ex - optimized bubble sort, insertion

**online** - all data to be sorted need not be loaded in memory at the same time

**external sorts** - sorts massive amount of data; all data to be sorted can't be loaded in memory at the same time

**comparison based sorts** - bubble, merge, etc.

**index based sorts** - count, radix

Mathematical upper bound in the worst case of any possible comparison-based sort is `O(n logn)`.

### Interesting Facts
- C++ `sort()` uses a hybrid sort called "Intro Sort" which is a combination of Insertion Sort, Quick Sort, and Heap Sort.
