### Priority Queues
Each element has a priority value attached to it that is used to determine access order of the elements.

Represented by:
- array of `pair<element, priority>`
- LL of `pair<element, priority>` (when there are more writes)
- heap
- BST

_Ref_: https://www.geeksforgeeks.org/priority-queue-set-1-introduction/

### Heaps
Must be Complete Binary Tree (to represent using Arrays and not to waste space) ans follow heap property (see below).

Types: 
- Max Heap (root is greater than both left and right)
- Min Heap (root is lesser than both left and right)

Upon insertion, travel upwards to verify if heap property is satisfied for each parent node, otherwise swap to make it so
```cpp
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

**Time Complexity**: Using Binary Heaps reduces time to `O(n * log k)` for Order Statistics queries since we sort only till the required part (`k` elements) and not the whole array `O(n * log n)`.

_Ref_: https://www.geeksforgeeks.org/binary-heap/
