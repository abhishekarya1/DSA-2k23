### Priority Queues
Each element has a priority value attached to it that is used to determine access order of the elements. On a tie, insertion order FIFO is followed.

Represented by:
- array of `pair<element, priority>`
- LL of `pair<element, priority>` (for write-heavy)
- Binary Heap (a Complete BT stored in Array)
- BST

Standard Operations:
- getMax/getMin (equivalent to peek)
- extractMax/extractMin
- insert
- delete (make it INT_MIN using decrease and extractMin)
- decrease (modify a key's value in-place)

_Ref_: https://www.geeksforgeeks.org/priority-queue-set-1-introduction/

### Heaps
Must be Complete Binary Tree (to represent using Arrays and not to waste space) and follow heap property (see below).

Types: 
- Max Heap (root is greater than both left and right)
- Min Heap (root is lesser than both left and right)

```cpp
root = heapArr[0]
parent(i) = heapArr[(i - 1) / 2]
left(i) = heapArr[2 * i + 1]
right(i) = heapArr[2 * i + 2]
```

Upon insertion, new node is inserted at the end of CBT. Travel upwards to verify if heap property is satisfied for each parent node, otherwise swap to make it so
```cpp
// min-heap
while (i != 0 && heapArr[parent(i)] > heapArr[i]){
       swap(heapArr[i], heapArr[parent(i)]);
       i = parent(i);
}
```

Upon deletion, use recursive **heapify** `O(log n)` method to satisfy heap property; this method assumes that the subtrees are already heapified
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

_Ref_: https://www.geeksforgeeks.org/binary-heap/

### Time Complexity
```cpp
Get - O(1); constant time access

Heapify - O(log n); traverses entire height of the tree which is log n

Extract - O(log n); calls heapify afterwards

Insertion - O(log n)

Deletion - O(log n); calls heapify afterwards

Decrease - O(log n); calls heapify afterwards

Build a heap with n elements: O(n log n); on every element insertion there will be a heapify
```

Using Binary Heaps reduces time to `O(n * log k)` for Order Statistics queries since we sort only till the required part (`k` elements) and not the whole array `O(n * log n)`.

### C++ STL
Top element is greatest by default in the Max Heap in C++.
```cpp
priority_queue <int> pq;       // Max Heap

priority_queue <int, vector<int>, greater<int>> pq;       // Min Heap
```

In Java, collection Priority Queue is by default a Min Heap.

## Problems
- Kth Largest/Smallest: for Kth largest use maxHeap and remove top `k-1` elements, vice-versa for Kth smallest. TC = `O(n logn + (k-1) log n)`
  - alternatively, we can use Min Heap for the Kth largest element, push elements into it, and never let it become size `> k`. TC = `O(k + (n-k) log k)`
- sort K (nearly sorted) sorted array: `O(n log k)`; sort only k elements from i (till `i + k`th position) to get element at `i`, use Min Heap and get top (min element) for every `i`, and move window by one to the right each step
- Merge K sorted Lists: Brute = sort `O(n log n)`, Optimized = use Min Heap to sort only k heads of all LL at a time `O(n log k)`
