> Linked Lists are _non-contiguous_ sequence of elements (called _nodes_ here).
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

Traversal Conditions: `while(curr != NULL)` or `while(curr -> next != NULL)` (skips last node).

_Note_: NULL checks are always done for nodes on which we're performing `-> next` operation e.g. `fast && fast -> next` in Hare & Tortoise technique.

Create gap of `k` nodes - useful in many problems

Reverse traversal of a LL - useful in many problems

## Basics
**Insert/Delete a node in SLL/DLL**:
- insertion/deletion of `head` is always tricky since we need to move the `head` pointer itself and also we can't do `prev -> next = curr -> next` if `curr` is at the head and it is the node to be deleted, `prev` is set to `NULL` or `head` and we won't be able to delete head (so when deletion range criteria is met we perform a check for head and shift head accordingly if its to be deleted; similarly for insertion).
- for all intermediate nodes in range, we can move `insertIndex - 1` or `deleteIndex - 1` times, check current node isn't `nullptr`, and re-point normally. Or use 2 pointers `prev` and `curr` for deletion (unnecessary tbh).

_Note_: always form link between new node and next node first, before breaking link between current and next.

**Reverse a SLL**: in-place reversal
- iterative (uses 3 pointers): save `next` node, update `curr->next = prev`, update `prev` and then `curr`, return the new head i.e. the last `prev` value
- recursive: go till end and while coming back with recursion, update link for current node (`curr->next->next = curr; curr->next=NULL;`), and propagate returned `newHead` from the base case to every recursive call's return.

**Delete node to which pointer is given**: ([link](https://leetcode.com/problems/delete-node-in-a-linked-list)) copy data and pointer of next node to current.

So basically deletion of a node is possible in two ways - if we know its previous node (actual deletion of memory address), or if we've the node itself (deletion of node data only), the latter is useful in deletion of middle node, `k`th node from end etc but may not be the ask.

## Fast and Slow Pointers
**Find middle of a LL**: Hare & Tortoise technique: `while(fast && fast -> next)`

**Detect loop (Floyd's cycle detection algorithm)**: ([notes](https://cp-algorithms.com/others/tortoise_and_hare.html)) use Hare & Tortoise technique. If a cycle is present, then they'll definitely meet after a finite number of steps. Otherwise, `fast` will reach the end. Note that the meeting point may or may not be the start point of the cycle.

**Find the starting point of cycle**: move simultaneously from meet point of `slow` and `fast` and the head of LL, answer is when they point to the same node, algebraic proof below:
```txt
time = dist / speed, in the same time they cover diff dist since speeds are diff (2 times the other)
so dist covered will also be twice (directly proportional), as time is constant for both

2 * Ds = Df
=> 2 (x + y)  = x + y + z + y
=> x = z
```

**Length of the cycle**: find cycle start point, count till it is encountered again.

**Floyd's Cycle Detection Algorithm** can be used for other applications apart from LL loops too:
- [Happy Number](https://leetcode.com/problems/happy-number): both pointers are guaranteed to meet. Upon meeting, both will be `1` (happy) or some other value (not happy).
- [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number): if elements values represent address then two elements will point to same address (i.e. duplicates), which is possible only if a loop is present.

**Find intersection point of two LL**:
- Brute (quadratic): for every node in `listA` check every node of `listB`.
- Better (linear space): store any one list's nodes in `set<Node*>`, for every node in the other list search the `set<>` for a match.
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

**Delete Kth node from the end**: corner case is when `k` is equal to list's size e.g. `list = [1, 2] and k = 2`, in this case during offsetting fast pointer will become `NULL` and we can return `head -> next` as new head
- offset by `k+1` and we'll land at previous node of the one to be deleted.
- offset by `k` nodes and track `prev` and repoint upon reaching node to be deleted.
- offset by `k` and reach node to be deleted and then copy data of its next and delete the next one.

```cpp
// edge case: [1], n = 1
if(head -> next == NULL) return NULL;

// edge case: n = size of array (removing head), ex - [1, 2, 3] (n = 3)
// offset by n steps and then check if we've reached the end (nth node from the end is head)
if(fast == NULL) return head -> next;
```

**Delete middle node**: go to middle node using hare-and-tortoise and track `prev` of `slow`. Re-point as usual `prev->next = slow->next` and then `delete slow`. This covers case like `[1,2]` where mid is element `2`.
- if not using `prev` but copying next node to current for deletion (`slow->val = slow->next->val`), `slow->next` maybe `NULL` after `slow` is at middle node in case `[1,2]`, so we need to check `slow->next == NULL` and modify `head->next = NULL` accordingly.

> [!TIP]
> When performing insertion or deletion operation of a node, edge case is always performing operation at `head`.

## Rearrangement
**Check if LL is palindrome**: ([link](https://leetcode.com/problems/palindrome-linked-list)) go to the middle node using rabbit & hare technique, reverse the right half in-place, compare one-by-one till end, reverse later to return to original list.
- if LL has odd no. of nodes, then list `[1, 2, 3, 2, 1]` will become `[1 -> 2 -> 3 <- 2 <- 1]` with `3 -> NULL` i.e. `3` becomes the common node, we can come from any side and `3` will be its own counterpart and will always be anyways equal to itself.
- if LL has even no. of nodes, then the rightward list (reverse list) will be slightly shorter (due to mid being the right one of the middle two elements), so always traverse in reverse list to compare.
- alternatively, just use condition `while (head != NULL && newHead != NULL)` and no need to understand all the above intricacies.

Similar Problem:
[Reorder List](https://leetcode.com/problems/reorder-list): reverse from mid to end just like above and use two head pointers and a `temp` variable to repoint as asked.
```cpp
while (second) {                      // since reversed half is shorter
    ListNode* temp = first->next;     // save
    
    first->next = second;
    second = second->next;
    first->next->next = temp;        // smart
    
    first = temp; 
}
```

**Segregate alternate nodes in LL**: ([link](https://leetcode.com/problems/odd-even-linked-list/)) track `oddTail = head` and `evenTail = head -> next` (and save it too `evenHead = evenTail` for later) and smartly re-point nodes in the same LL i.e. `evenHead->next` goes in odd list and `evenHead->next->next` goes in even list. At the end attach both LLs with `oddTail -> next = evenHead`.

**Segregate odd and even value nodes** and **Segregate nodes with values 0, 1, and 2**: similar to above; use `oddHead`, `oddTail` and `evenHead`, `evenTail` and attach nodes to them like Legos based on their `val`.

**Merge two sorted LL**: ([link](https://leetcode.com/problems/merge-two-sorted-lists)) create a `dummyHead` node and keep pointing its next to lesser value node (use a `mergeTail` pointer to track last node in merged LL), also attach remaining lists at the end (replacement for `while` loops in array merge technique), at the end return `dummyNode -> next` as the new head of the merged LL.

**Sort LL**: ([link](https://leetcode.com/problems/sort-list)) use either bubble sort and swap node data, or use merge sort for LL
- _Merge sort for LL_: split in the middle and call mergeSortLL on both halves, merge using a dummy node and attach Legos technique.

**Remove duplicates from a sorted LL**: ([link](https://leetcode.com/problems/remove-duplicates-from-sorted-list)) same as array problem, using two-pointers, but here we can just re-point instead of swapping. Also seal the end `j -> next = NULL` to handle duplicates at the end e.g. `[1,2,3,3,3]`.

**Delete all nodes with value K**: ([link](https://leetcode.com/problems/remove-linked-list-elements/)) normal deletion using two pointers `prev = head` and `curr = head`, normal case is fine but head deletion is a problem in cases like `[2], k = 2` and `[6, 6, 6, 6], k = 6`.
- Scan Delete Approach: `prev` won't move on deletion here only `curr` will, both move on non-deletion. `curr == head` case needs to be checked on every step as in that case `head` itself needs to be shifted (`head = head -> next`) unlike the normal case.
- Dummy Node Approach: create a dummy node and attach entire list head to it, init `*prev = dummy` and `*curr = head`, skip `curr -> val == k` nodes in traversal using `prev` and `curr` logic from above approach and repoint, this way we won't have to deal with head check on deletion case, return `dummy -> next` at the end.

## Numbers represented by LL
> Reverse the LL first in all problems of this kind.

**Add 1 to a number represented by LL**: reverse LL and while carry is more than `0`, keep adding, add node at last if carry remains. Reverse entire list again to get ans.

**Add two numbers represented by LL**: ([link](https://leetcode.com/problems/add-two-numbers)) lists are already reversed (otherwise reverse), add corresponding node data `while(h1 && h2)` with carry propagation logic, do `while(h1)` and carry prop logic (num1 is longer processing), do `while(h2)` and carry prop logic (num2 is longer processing), carry can still remain after this too so create and add a node with `newNode -> data = carry`, at the end `return dummyNode -> next` (skip dummy node).

## Doubly Linked List (DLL)
**Reverse a DLL**: swap links and goto `curr->prev` node (next node in original list), place new head at the end (use a `prev` variable or condition `if(curr->prev == NULL) head = curr;` to place new head).

**Find all pairs with given sum in DLL**: same as array two pointer, just the condition is diff because there are no numeric indices here to compare. Condition - `while(low != hi && hi -> next != low)`, the second half is for the state `[2, 3]` with `k = 5`, the two pointers will update by `1` position each and will cross each other without ever becoming equal, that is where we should stop.

**Delete all nodes with value K in DLL**: Same as in SLL but re-pointing needs to be done for nodes's `prev` pointer too.

## In-place Reversal
- Reverse LL in groups of k: 
  - Iterative way: to reverse k nodes, `k-1` links are reversed, use (iterative 3-pointer link reverse technique) that starts at `dummyNode` (important) and can reverse links without changing `prev` or `curr` and connects segments properly, do this `while(n >= k)`
  - NOTE that normal reverse approach won't work here since we will reverse `1 2 3 4` with `k = 2` as `2 1 3 4` (first link reversal) and then we'll need to attach node `1` to next block's last node which is a hassle since first we'll have to reverse next block and then connect them, dummy node approach is much smarter
  - Recursive way: reverse first k nodes iteratively in method, and let recursion do for the rest of the list and attach to recursive call's return, return (propagate) last node of block back everytime. Base case: `when(lengthOfLL < k)` then no reversal to be done

- Rotate a LL: make it circular and break

- Flattenning of a LL: recur till last and when coming back form pairs and merge sorted sublists like normal

Clone a LL with random and next pointer

---

- Remove Nodes till next Greater Node - we do it the reverse LL way, and we connect current to next greater and propagate back the greater node between current and greater [link](https://leetcode.com/problems/remove-nodes-from-linked-list/)

- Split Linked List in Parts - `n/k` node in each part but the first `n%k` parts have `1` extra node each (`n/k + 1`)

## Additional Topics
**XOR Linked Lists** - ([notes](https://hashdefine.netlify.app/dsa/bit/applications/#xor-linked-lists)) space-efficient Linked Lists in which two-way traversal is possible with only one pointer storage per node.

**Reservior Sampling**: Linked Lists are prefect data structure for this kind of Randomized Algorithm - [notes](https://github.com/abhishekarya1/DSA-2k23/blob/main/maths.md#randomized-algorithms---reservior-sampling).

**Skip Lists**: ([link](https://www.geeksforgeeks.org/dsa/skip-list/)) time-efficient Linked List. Probabilistic data structure that allows for on average `O(logn)` time operations due to "express lanes" made by "skip" pointers to non-adjacent nodes further in the list. We can have multiple layers of skip pointers on top of the base LL. These layers and nodes in each such layer is determined randomly, hence making this data structure "probabilistic".

**Multilevel Lists**: ([link](https://www.geeksforgeeks.org/dsa/multilevel-linked-list/)) Each node can have child pointers as well allowing for a "mesh-like" structure connecting multiple lists. Ex - DOM trees, Hierarchical Page Tables in OS, Indirect FS blocks (Unix inodes), Multilevel Indexes in DB, etc.
