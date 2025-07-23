> Linked Lists are _non-contiguous_ sequence of elements.
> 
> Dynamic size, and elements are accessed sequentially so random access to elements is not possible.

**Time Complexities**:
- Insertion at end: `O(n)` (due to sequential access traversal)
- Insertion at start: `O(1)`
- Deletion at start: `O(1)`
- Deletion at end: `O(n)` (due to sequential access traversal)
- Accessing a random element: `O(n)` (due to sequential access traversal)

**Pros**: 
- fast writes (LRU), slower reads and random access. We can decrease operation times with `tail` pointer or by using DLL.
- better for systems that may not allocate memory sequentially (fragmented memory).
- no unused allocated space (unlike arrys that may have gaps).

## Tips
Base Case / Edge Case is if LL has no nodes or only one node: `if(head == NULL || head -> next == NULL)`.

Traversal Conditions: `while(curr != NULL)` or `while(curr -> next != NULL)` (skips last element).

_Note_: NULL checks are always done for nodes on which we're performing `-> next` operation e.g. `fast && fast -> next` in Hare & Tortoise technique.

Create gap of `k` nodes - useful in many problems

Reverse traversal of a LL - useful in many problems

## Basics
**Insert/Delete a node in SLL/DLL**:
- insertion/deletion of `head` is always tricky since we need to move the `head` pointer itself and also we can't do `prev -> next = curr -> next` if `curr` is at the head and it is the node to be deleted, `prev` is set to `NULL` or `head` and we won't be able to delete head (so when deletion range criteria is met we perform a check for head and shift head accordingly if its to be deleted; similarly for insertion).
- for all intermediate nodes in range, we can move `insertIndex - 1` or `deleteIndex - 1` times, check current node isn't `nullptr`, and re-point normally. Or use 2 pointers `prev` and `curr` for deletion (unnecessary tbh).

_Note_: always form link between new node and next node first, before breaking link between current and next.

**Reverse a DLL**: swap links and goto `curr->prev` node (next node in original list), place new head at the end (ue a `prev` variable or condition `if(curr->prev == NULL) head = curr;` to place new head).

**Reverse a SLL**: in-place reversal
- iterative (uses 3 pointers): save `next` node, update `curr->next = prev`, update `prev` and then `curr`, return the new head i.e. the last `prev` value
- recursive: go till end and while coming back with recursion, update link for current node (`curr->next->next = curr; curr->next=NULL;`), and propagate returned `newHead` from the base case to every recursive call's return.

**Delete Linked List Nodes with value K**: since its deletion we use two pointers `*prev = head` and `*curr = head`, normal case is fine but head deletion is problem in cases like `[2], k = 2` and `[6, 6, 6, 6], k = 6` [link](https://leetcode.com/problems/remove-linked-list-elements/)
- Scan Delete Approach - `prev` won't move on deletion here only `curr` will, both move on non-deletion. `curr == head` case needs to be checked on every step as in that case `head` itself needs to be shifted (`head = head -> next`) unlike the normal case
- Dummy Node Approach - create a dummy node and attach entire list head to it, init `*prev = dummy` and `*curr = head`, skip `curr -> val == k` nodes in traversal using `prev` and `curr` logic from above approach, this way we won't have to deal with head check on deletion case, return `dummy -> next` at the end

**Delete node to which pointer is given**: ([link](https://leetcode.com/problems/delete-node-in-a-linked-list)) copy data and pointer of next node to current.

So basically deletion of a node is possible in two ways - if we know its previous node (actual deletion of memory address), or if we've the node itself (deletion of node data only), the latter is useful in deletion of middle node, `k`th node from end etc but may not be the ask.

## Fast and Slow Pointers
**Find middle of a LL**: Hare & Tortoise technique: `while(fast && fast -> next)`

**Detect loop (Floyd's cycle detection)**: use Hare & Tortoise technique. If a cycle is present, then they'll definitely meet after a finite number of steps.

**Find the starting point of cycle**: move simultaneously from meet point of `slow` and `fast` and the head of LL, answer is when they point to the same node, algebraic proof below:
```txt
time = dist / speed, in the same time they cover diff dist since speeds are diff (2 times the other)
so dist covered will also be twice (directly proportional), as time is constant for both

2 * Ds = Df
=> 2 (x + y)  = x + y + z + y
=> x = z
```

**Length of the cycle**: find cycle start point, count till it is encountered again

**Find intersection point of two LL**:
- Brute (quadratic): for every node in `listA` check every node of `listB`.
- Better (linear space): store any one list's elements in `set<Node*>`, for every node in the other list search the `set<>` for a match.
- Good: calc size diff of LL from both heads (`diff`), move by `diff` steps in the longer one, then start traversal simultaneously in the smaller LL, where they meet is the common point.
- Optimal (super smart): start traversing from `h1` and on end circle back to `h2` and vice-versa, after/in the second traversal it is guaranteed that you will stop at either `NULL` (trivial common point) or the answer node.

**Find the Kth node from the end**: give headstart of `k` steps to `fast`, move both `slow` and `fast` one step at a time until fast reaches the end.
```cpp
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
// alternatively we can use a prev pointer and stop normally at kth position
```

**Delete Kth node from the end**: 
- offset by `k+1` and we'll land at previous node of the one to be deleted.
- offset by `k` nodes and track `prev` and repoint upon reaching node to be deleted, corner case is when `k` is equal to list's size e.g. `list = [1, 2] and k = 2`, in this case during offsetting fast pointer will become `NULL` and we can return `head -> next` as new head (meaning deletion of `head`).
- offset by `k` and reach node to be deleted and then copy data of its next and delete the next one (beware of corner cases).

**Delete middle element**: goto mid element using hare and tortoise, corner case is two element list e.g. `[1, 2]`, mid is `2`, for this when slow is on mid and `slow -> next == NULL` set `head -> next == NULL` and return head.

---

- Check if LL is palindrome or not: goto mid using rabbit & hare technique, reverse the right half, compare one-by-one till end
- Segregate odd and even nodes in LL: track `oddHead = head` and `evenHead = head -> next` (and save this `evenStartSave = evenHead` for later) and re-attach nodes from LL like Legos

```cpp
// edge case - [1], n = 1
if(head -> next == NULL) return NULL;

// edge case - n = size of array (removing head), ex - [1, 2] (n = 2)
// offset by n steps and then check if we've reached the end (nth node from the end is head)
if(fast == NULL) return head -> next;
```

---

- Segragate odd and even nodes & segragate 0s, 1s and 2s - attach nodes like Lego bricks

- Sort Linked list: use either bubble sort and swap node data, or use merge sort for LL

- Merge sort for LL - split in the middle and call mergeSortLL on both halves, merge using a dummy node and attach legos

- Merge two sorted LL - take a dummyHead node and keep pointing it to lesser value node

- Add 1 to a number represented by LL: reverse LL and while carry is more than `0`, keep adding, add node at last if carry remains

- Add two numbers represented by LL: LL are already reversed (otherwise reverse), add corresponding node data `while(h1 && h2)` with carry propagation logic, do `while(h1)` and carry prop logic (num1 is longer processing), do `while(h2)` and carry prop logic (num2 is longer processing), carry can still remain after this too so create and add a node with `newNode -> data = carry`, at the end `return dummyNode -> next` (skip dummy node)

---

- Reverse LL in groups of k: 
  - Iterative way: to reverse k nodes, `k-1` links are reversed, use (iterative 3-pointer link reverse technique) that starts at `dummyNode` (important) and can reverse links without changing `prev` or `curr` and connects segments properly, do this `while(n >= k)`
  - NOTE that normal reverse approach won't work here since we will reverse `1 2 3 4` with `k = 2` as `2 1 3 4` (first link reversal) and then we'll need to attach node `1` to next block's last element which is a hassle since first we'll have to reverse next block and then connect them, dummy node approach is much smarter
  - Recursive way: reverse first k nodes iteratively in method, and let recursion do for the rest of the list and attach to recursive call's return, return (propagate) last node of block back everytime. Base case: `when(lengthOfLL < k)` then no reversal to be done

- Rotate a LL: make it circular and break

- Flattenning of a LL: recur till last and when coming back form pairs and merge sorted sublists like normal

---

- Find pairs with given sum in DLL: same as array two pointer just condition is diff (`while(low != hi && hi -> next ! = low)`)

- Delete nodes of a DLL: take care of edge cases - deletion of first node, deletion of last node

- Remove Nodes till next Greater Node - we do it the reverse LL way, and we connect current to next greater and propagate back the greater element between current and greater [link](https://leetcode.com/problems/remove-nodes-from-linked-list/)

- Split Linked List in Parts - `n/k` element in each part but the first `n%k` parts have `1` extra element each (`n/k + 1`)
