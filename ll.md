- Base Case: `if(head == NULL || head -> next == NULL)`
- Traversal: `while(curr != NULL)` or `while(curr -> next != NULL)` (skips last element)

---
- Deleting a node in a SLL: use 2 pointers, `prev curr`
- Delete node to which pointer is given: copy data of next node to current
- Reverse a SLL: Iterative (uses 3 pointers): save `next` node, update `curr->next = prev`, update `prev` and then `curr`, return the new head i.e. the last `prev` value
  - Recursive way: propagates returned `head` from the base case
- Reverse a DLL: swap links and return new head at the end i.e. `prev`
- Find middle of a LL: Hare & Tortoise approach
  - `while(fast && fast->next)`
- Detect loop (Floyd's cycle): Hare & Tortoise approach with Fast and Slow pointers
- Find the starting point in LL: `x y` algebraic way, move simultaneously from meet point of `slow` and `fast` and the head of LL, answer is when they point to the same node
- Length of the loop: find cycle start point, count till it is encountered again
- kth node from the last: give headstart of `k-1` steps to `ptr2`, move both `ptr1` and `ptr2` one step at a time
 
---
- Check if LL is palindrome or not: goto mid, reverse the right half, compare one-by-one
- Segregate odd and even nodes in LL: track `oddHead evenHead oddEnd evenEnd` and attach nodes from original LL like Legos
- Delete Nth node from the last: goto n+1th node from the last and change links to nth node, corner case is `list = [1, 2] and n = 2`, in this case when we goto n+1th node, ahead pointer will be `NULL` and we can return `head -> next` as new head
- Delete middle element: goto mid element using hare and tortoise, corner case is `[1, 2]`, mid is `2`, for this on `slow -> next == NULL` set `head -> next == NULL` and return head
- Find intersection point of two LL:
  - calc size diff of LL from both heads (`diff`), move by `diff` steps in the long one, traverse simultaneously in the smaller LL, where they meet is the common point
  - start traversing from `h1` and on end circle back to `h2` and vice-versa, after 2 taversals it is guranteed that you will stop at `NULL` (common point) or the answer node before that
 
