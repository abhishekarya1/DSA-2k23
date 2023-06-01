- Selection Sort (Greedy)
- Bubble Sort (+ adaptive optimization + recursive)
- Insertion Sort
- Merge Sort (Divide & Conquer)
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
**comparison based sorts** - bubble, merge, etc...
**index based sorts** - count, radix

Mathematical worst-case of any possible comparison-based sort is `O(n logn)`.
