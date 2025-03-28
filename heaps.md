## Priority Queue
Each element has a priority value attached to it that is used to determine access order of the elements. On a tie, insertion order FIFO is followed.

Represented by:
- Array of `pair<element, priority>`
- LL of `pair<element, priority>` (for write-heavy)
- Binary Heap (a complete BT stored in array)
- BST

Standard Operations:
- getMax/getMin (equivalent to peek)
- extractMax/extractMin
- insert(k)
- delete(i) (make it `INT_MIN` using decrease and extractMin)
- decreaseKey(i,k) (modify a key's value in-place)

_Ref_: https://www.geeksforgeeks.org/priority-queue-set-1-introduction/

## Heap
Must be Complete Binary Tree (to represent using Arrays and not to waste space) and follow heap property (see below).

Types: 
- Max Heap (root is greater than both left and right; recursively)
- Min Heap (root is lesser than both left and right; recursively)

```cpp
// Heap ADT class and constructor
MinHeap::MinHeap(int cap) { 
    heap_size = 0; 
    capacity = cap; 
    heapArr = new int[cap]; 
}
```

```txt
root = heapArr[0]
parent(i) = heapArr[(i - 1) / 2]
left(i) = heapArr[(2 * i) + 1]
right(i) = heapArr[(2 * i) + 2]
```

### Two Kinds of Heapify
- Sift Up / Bubble Up / Bottom Up - used in `insertion`
- Sift Down / Bubble Down / Top Down - used in `deletion`, building Min-Heap from Max-Heap (re-structuring)

Both are NOT equivalent as they don't result in the same output as Sift Up only considers path from a newly inserted node to entire heap's root as we are inerting in an already existing Heap, so third element needn't be compared as it already is lesser than its root. Both have TC = `O(log n)`

Upon insertion, new node is inserted at the end of CBT. Travel upwards (**Sift Up**) to verify if heap property is satisfied for each parent node, otherwise swap to make it so; assumes that the subtrees already satisfy heap property
```cpp
// min-heap - iterative sift up heapify

while (i != 0 && heapArr[parent(i)] > heapArr[i]){
       swap(heapArr[i], heapArr[parent(i)]);
       i = parent(i);
}
```
```py
# min-heap - recursive sift up heapify

def siftUp(heapArr, i):
    parent = (i - 1) // 2
    if parent < 0:
        return
    if heap[i] < heap[parent]:
        heapArr[i], heapArr[parent] = heapArr[parent], heapArr[i]    # swap
        siftUp(heapArr, parent)                                      # sift up to the parent
```

Upon deletion, root node (min/max) is removed by replacing it with the last element of CBT (rightmost leaf) and heapify-ing (**Sift Down**). Use recursive **heapify** `O(log n)` method to satisfy heap property from top to bottom (node `i` to leaves); comparison here happens among all 3 nodes of the smallest unit subtree so this is more widely used than Sift Up heapify
```cpp
void minHeapify(int i){
    int l = left(i);
    int r = right(i);
    int smallest = i;
    if (l < heap_size && heapArr[l] < heapArr[i])
        smallest = l;
    if (r < heap_size && heapArr[r] < heapArr[smallest])
        smallest = r;
    if (smallest != i){
        swap(heapArr[i], heapArr[smallest]);
        minHeapify(smallest);
    }
}
```

```txt
Summary of Heap Operations:

1) getMin - return heapArr[0]
2) insert(k) - check overflow, insert at end, satisfy heap property recursively at i
3) extractMin - return heapArr[0], replace heapArr[0] = heapArr[heap_size - 1], heapify at 0
4) decreaseKey(i, k) - heapArr[i] = k, satisfy heap property recursively at i (assumed that k is less than heapArr[i])
5) delete(i) - decreaseKey(i, INT_MIN), and then do extractMin
```

_Ref_: https://www.geeksforgeeks.org/binary-heap/

### Time Complexity
```txt
Get - O(1); constant time access

Heapify - O(log n); traverses entire height of the tree which is log n

Extract - O(log n); calls heapify afterwards

Insertion - O(log n); satisfy heap property recursively

Deletion - O(log n); calls Extract afterwards which calls heapify

DecreaseKey - O(log n); satisfy heap property recursively

Build a heap with n elements: O(n * log n); on every element insertion there will be a heapify
```

The time complexity of the `Extract` operation is `O(log n)`, but `n` is the size of the heap and it keeps on decreasing as we keep on extracting elements from the heap. We consider the asymptotic upper-bound so we consider time for eacg operation as `log(n)` but the actual runtime of the operation is potentially lower than that.

Using Binary Heaps (atmost 2 children of each node) reduces time to `O(n * log k)` for Order Statistics queries since we sort only till the required part (`k` elements) and not the whole array `O(n * log n)`

### C++ STL
Top element is greatest by default in the Max-Heap in C++
```cpp
priority_queue <int> pq;       // Max-Heap

priority_queue <int, vector<int>, greater<int>> pq;       // Min-Heap
```

In Java Collection, class `PriorityQueue` implements `Queue` interface, and is by default a Min-Heap

### Usage of PQ
- Kth Order Statistics - find top/bottom `k` elements
- Heap Sort - comparison based sorting algorithm
- BFS Traversal of Graphs and Trees (level-order traversal)
- Dijkstra's Algorithm
- Prim's Algorithm
- Huffman Coding

## Problems
**Build Max-Heap from Min-Heap**: call maxHeapify (Sift Down - 3 node comparison) for all nodes starting from rightmost bottom node doing reverse traversal of the heap array. Start from the last root (`(n-1)-1/2`) or we can start with node `n-1` too and conditions in the heapify method will take care of bounds

**Kth Largest/Smallest Element**: 
- Intuitive Approach: for Kth largest build Max-Heap from all elements and remove top `k-1` elements, vice-versa for Kth smallest. TC = `O(n logn + (k-1) log n)`, SC = `O(n)`
- Better Approach (better TC and SC): we can use Min-Heap for the Kth largest element, keep pushing elements into it and if heap size becomes `> k` pop the top to make size `== k`. At the end, Min-Heap's top will be the ans. TC = `O(n log k + log k)`, SC = `O(k)`
    - summary: add `k+1`th element to heap and pop top to maintain `k` sized heap

**Sort K (nearly sorted) Sorted Array**: `O(n log k)`; use Min Heap and get top (min element) for every `i` from among its next `k` elements, do this for every `i < n`, start with a heap built from the first `k + 1` elements of the array (to find correct element for `i = 0`), then pop one from heap and insert one from array, when array traversal is done, copy elements to array from heap until heap becomes empty

**Merge K Sorted Lists**: 
- Brute Force - put nodes in array and sort them by data (using comparator) and attach adjacent nodes in array. TC = `O(n log n)`, SC = `O(n)` where `n` is the total nodes in all `k` LLs
- Better#1 - merge LLs in pairs of two, a total of `k-1` merges are required. TC = `O(k-1 * n*k)` where `n` is avg node per LL
- Better#2 - merge LLs using `k` pointers, like above approach but in one go. TC = `O(k * n*k)`, SC = `O(k)` where `n` is avg node per LL
- Optimal-  use Min-Heap `pq<int, Node*>` to get min among all current nodes of LLs at a time and move ahead in the same LL (insert min node's next) from which min is extracted to maintain heap size as `k`. TC = `O(n log k)`, SC = `O(k)` where `n` is total nodes in all LLs

**Replace Array Element with its Rank in Sorted Array**: be careful of duplicates distorting rank, because for them index of element in sorted array + 1 will not give their rank! Ex - `[2, 2, 2, 3]` ans is `[1, 1, 1, 2]`
- Brute Force#0 (_naive thinking; works only if no duplicates are present_) - create another duplicate array and sort it, use linear search to find rank of elements in the original array. TC = `O(n + n log n + n*n)`, SC = `O(n)`
- Brute Force - store all elements smaller than current in a `set` and set size + 1 is current's rank, store in `ans` array. TC = `O(n*n)`, SC = `O(n + n)`
- Better - do the same as the above approach but store element to its rank (track using a `rank` variable) mapping in an `unordered_map` only if not seen previously `== 0`, TC = `O(n + n log n + n*1)`, SC = `O(n + n)`
- Optimal - use heap
