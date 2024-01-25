- Base Case: `if(head == NULL || head -> next == NULL)`
- Traversal: `while(curr != NULL)` or `while(curr -> next != NULL)` (skips last element)

---
- Deleting a node in a SLL: use 2 pointers, `prev curr`
- Delete node to which pointer is given: copy data of next node to current
- Reverse a SLL: Iterative (uses 3 pointers): save `next` node, update `curr->next = prev`, update `prev` and then `curr`, return the new head i.e. the last `prev` value
  - Recursive way: go till end and while coming back with recursion, change and break link, and propagate returned `newHead` from the base case
- Reverse a DLL: swap links and return new head at the end i.e. `prev`
- Find middle of a LL: Hare & Tortoise approach
  - `while(fast && fast->next)`
- Detect loop (Floyd's cycle): Hare & Tortoise approach with Fast and Slow pointers
- Find the starting point in LL: move simultaneously from meet point of `slow` and `fast` and the head of LL, answer is when they point to the same node, algebraic proof below:
```txt
time = dist / speed, in the same time they cover diff dist and diff speeds
2 (x + y)  = x + y + z + y
x = z
```
- Length of the loop: find cycle start point, count till it is encountered again
- kth node from the last: give headstart of `k` steps to `fast`, move both `slow` and `fast` one step at a time
```txt
// move fast pointer k steps ahead
Node* slow = head, *fast = head;
while(k--) fast = fast -> next;

// stop at kth node from the end
while(fast){
  slow = slow -> next;
  fast = fast -> next;
}

// stop at k+1th node from the end (useful to delete kth node)
while(fast -> next){
  slow = slow -> next;
  fast = fast -> next;
}
```

---
- Check if LL is palindrome or not: goto mid using rabbit & hare technique, reverse the right half, compare one-by-one till end
- Segregate odd and even nodes in LL: track `oddHead = head` and `evenHead = head -> next` (and save this `evenStartSave = evenHead` for later) and re-attach nodes from LL like Legos
- Delete Nth node from the last: offset by `n` nodes and then goto one node previous to `n`th node from the last and change links to nth node, corner case is when `n` is equal to list's size e.g. `list = [1, 2] and n = 2`, in this case during offsetting fast pointer will become `NULL` and we can return `head -> next` as new head (meaning deletion of `head`)
- Delete middle element: goto mid element using hare and tortoise, corner case is two element list e.g. `[1, 2]`, mid is `2`, for this when slow is on mid and `slow -> next == NULL` set `head -> next == NULL` and return head
```txt
// edge case - [1], n = 1
if(head -> next == NULL) return NULL;

// edge case - n = size of array (removing head), ex - [1, 2] (n = 2)
// offset by n steps and then check if we've reached the end (nth node from the end is head)
if(fast == NULL) return head -> next;
```

- Find intersection point of two LL:
  - Naive: for every node in `listA` check every node of `listB` 
  - Better: calc size diff of LL from both heads (`diff`), move by `diff` steps in the longer one, traverse simultaneously in the smaller LL, where they meet is the common point
  - Optimal: start traversing from `h1` and on end circle back to `h2` and vice-versa, after 2 taversals it is guranteed that you will stop at either `NULL` (common point) or the answer node

---

- Segragate odd and even nodes & segragate 0s, 1s and 2s - attach nodes like Lego bricks
- Sort Linked list: use either bubble sort ans swap node data, or use merge sort for LL
- Merge sorted LL - take a dummyHead node and keep pointing it to lesser value node
- Add 1 to a number represented by LL: reverse LL and while carry is more than 0, keep adding, add node at last if carry remains
- Reverse LL in groups of k: 
  - Iterative way: to reverse k nodes, k-1 links are reversed, use modified iterative 3-pointer reverse and start at `dummyNode` (important)
  - Recursive way: reverse first k nodes and let recursion do for the rest of the list. Base case: `when(lengthOfLL < k)` then no reversal to be done
- Rotate a LL: make it circular and break
- Flattenning of a LL: recur till last and when coming back form pairs and merge sorted sublists like normal

--- 
- Find pairs with given sum in DLL: same as array two pointer just condition is diff (`while(low != hi && hi -> next ! = low)`)
- Delete nodes of a DLL: take care of edge cases - deletion of first node, deletion of last node
