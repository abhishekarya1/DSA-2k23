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

Using Binary Heaps reduces time to `O(n * log k)` for Order Statistics queries since we sort only till the required part (`k` elements) and not the whole array `O(n * log n)`

### C++ STL
Top element is greatest by default in the Max-Heap in C++
```cpp
priority_queue <int> pq;       // Max-Heap

priority_queue <int, vector<int>, greater<int>> pq;       // Min-Heap
```

In Java Collection, class `PriorityQueue` implements `Queue` interface, and is by default a Min-Heap

### Usage of PQ
- Kth Order Statistics
- Heap Sort - comparison based sorting algorithm
- BFS Traversal of Graphs and Trees (level-order traversal)
- Dijkstra's Algorithm
- Prim's Algorithm
- Huffman Coding

## Problems
**Build Max-Heap from Min-Heap**: call maxHeapify (Sift Down - 3 node comparison) for all nodes starting from rightmost bottom node doing reverse traversal of the heap array. Start from the last root (`(n-1)-1/2`) or we can start with node `n-1` too and conditions in the heapify method will take care of bounds

**Kth Largest/Smallest Element**: 
- Intuitive Approach: for Kth largest build Max-Heap from all elements and remove top `k-1` elements, vice-versa for Kth smallest. TC = `O(n logn + (k-1) log n)`
- Better Approach (better TC and SC): we can use Min-Heap for the Kth largest element, keep pushing elements into it and if heap size becomes `> k` pop the top to make size `== k`. At the end, Min-Heap's top will be the ans. TC = `O(k + (n-k) log k)`
    - summary: add `k+1`th element to heap and pop top to maintain `k` sized heap

**Sort K (nearly sorted) Sorted Array**: `O(n log k)`; sort only k elements from i (till `i + k`th position) to get element at `i`, use Min Heap and get top (min element) for every `i`, and move window by one to the right each step

**Merge K Sorted Lists**: Brute = sort `O(n log n)`, Optimized = use Min Heap to sort only k heads of all LL at a time `O(n log k)`
