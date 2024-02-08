### Stack and Recursion
- Sort stack using recursion
- Reverse stack using recursion

See [recursion notes](/recursion.md)

### ADT
- Stack using:
  - Arrays: insertion and deletion at end using `int top;` index
  - Linked List: insertion and deletion at head
- Queue using: 
  - Arrays: use circular inserts and deletion (`(rear + 1) % maxSize` and `(front + 1) % maxSize`) to avoid wastage of space, `front` points to the index of element to be deleted next and `rear` points to the last inserted element index. Maintain a `currSize` and update it on every push/pull to the queue, will need to reset `front = rear = -1` on deletion of the only remaining element since we point to actual elements with front and rear and none exists in an empty queue, upon first element insert do `front = rear = 0`
  - Linked List: insertion at tail, deletion at head

- Stack using Queue: bring recently added element to front by extracting and pushing front to rear `n-1` times
- Queue using Stack: have to use 2 stacks. Shift elements between stacks for either push or pull operation:
  - first approach: push shifts elements to aux stack and back to main stack (maintains rev order), pop can be `O(1)` or `O(n)` depending on element availability in `input` stack. Size of queue = input stack
  - second approach: pop shifts elements to second stack, no need to move back to second stack here (consequent pop can be constant then), pop can be `O(1)` or `O(n)` depending on element availability in `output` stack. Size of queue = input stack + output stack

### Classic Problems
**Check Balanced Parentheses**: push open brackets, pop if stack top is a match of current. Invalid cases:
  - when brackets at stack top and current don't match (bracket pairing mismatch)
  - if we encounter a closing bracket but stack is empty (extra closing bracket)
  - when we've reached till the end and stack is still not empty (extra opening bracket)

Intuition: latest bracket is always on the stack top and we close inner pairings first as we go, rest are also in order of appearance (i.e. previous latest at top)
  
**Min Stack**: retrieves minimum element in `O(1)` time while keeping stack in the order of insertion
- use two stacks - one main and one that stores all minSoFar encountered during insertions, during push and pop check minStack top and update both accordingly, SC = `O(2N)`
- use `stack<pair<element, minElementSoFar>>`, this is actually equivalent to the above approach, SC = `O(2N)`
- math approach (SC = `O(N)`): if we have a new minimum push `2*newMin - oldMin` into the stack, it is guaranteed that this value we are pushing is less than element we are pushing (also our new minimum) and this position is where we encountered a new minimum. While popping, we need to make sure that our minimum changes when we are popping the minimum element from the stack so we check for change point (`stackTop < minSoFar`) and then modify minimum accordingly by the formula used above [link](https://www.baeldung.com/cs/stack-constant-time)

Mathematical derivation:
```txt
newMin < oldMin                      - changepoint
newMin + newMin < oldMin + newMin    - add newMin to both sides
2*newMin - oldMin < newMin           - transposition (for identification of changepoint using minSoFar and stackTop)
stackTop < newMin                    - push to stack and update minSoFar = newMin
```

### Prefix, Infix, Postfix Expressions
```txt
Infix - (A+B)/C
Postfix - AB+C/

Infix - A+B/C
Postfix - ABC/+

Infix - A/B+C
Postfix - AB/C+
```

**Postfix to ANY**: scan from left to right, if operand then push onto to stack, if operator then pop 2 elements from stack and form infix/postfix for them and push them back to the stack, at the end there will only be one element in the stack and that is our answer

**Prefix to ANY**: scan from _right to left_ and do the same as above

**Infix to Postfix**: if operand then print it, if operator and its precedence is greater than stack top push it onto the stack, otherwise pop until stack top is of greater precedence than current (i.e. keep stack in order such that high precedence are on top), at the end print all stack elements if expression traversal is finished, always push `(` and on `)` pop all stack elements till `(` and discard both parenthesis

Intuition: Postfix will always have operands in order of appearance from left to right and more importantly operators in the order in which they are applied in Infix are always listed from left to right in Postfix. By maintaining precedence of operations in the stack, we're guaranteeing that property in resulting Postfix expression.

**Infix to Prefix**: reverse prefix expression, we're making it "nearly-postfix" by converting `(` to `)` and vice-versa, convert this using `infixToPostfix()` and store in `resultString`, reverse `resultString` again

### Monotonic Stack
**Next Greater Element**: we are creating a monotonic stack here and keep the top as NGE at all times, also the top element will always be the smalllest in the stack. Traverse the array from the right, and if top is greater than current, top is NGE, else pop out elments till we reach NGE (or empty) in the stack. Popping elments will make sure that current is the smallest in the stack, on pushing it is the top and stack remains monotonic.

Smart way to code the same - run while loop for pops first, if stack is not empty we have our NGE otherwise push element onto the stack

Apply to circular array (NGE 2) - do the same and find NGE from i = 2n-1 to 0 and use `i%n` to access elements, put in another `ans` array only when `i<n`

**Trapping Rain Water**:
  - water at i = minimum of (maximum at left and right) - level of i  --  time = `O(n^2)`
  - we can precompute and store leftMax and rightMax in two separate arrays -- time = `O(n)`, space = `O(2n)`
  - two pointer approach:
    - increment left pointer only when there is a greater element on the right pointer, and track leftMax, this way we can find water stored at i only using leftMax since we've reached i only because there is a greater element on the right and we don't even need it to calc water stored at i as `ans += leftMax - arr[i]`. Do the same for right pointer.
    - if we've reached here that means there is a greater element on the right i.e. `arr[hi]` and we don't need to use it for calc. Also `leftMax >= arr[lo] >= arr[hi]` at this point since all lows below this one had a greater element on the right, that's why `lo++` happened and we've reached this position

**Subarray minimum and maximum**: find all NGE, PGE, NLE, PLE for an element and store in pre-compute arrays (optimization). Then use combinatorics formual `(g1 + 1) * (g2 + 1)` to calculate no. of subarrays that have that particular element as min/max. To calc elements on the left/right from/to PLE, etc... requires deatiled and careful index handling.

**Remove k digits to make minimum number**: if the number's digits are monotonic, we can decide which to remove. We always remove left digit (most significant) but it has to be greater too. so we traverse from left to right and remove element from stack if an inversion is encountered. Edge cases - duplicates, `01200`, increasing sequence `1234` (remove last k digits from it at last)

**LRU Cache**: use DLL and `unordered_map<key, Node*>`. Use two dummy nodes `(-1, -1)` as `head` and `tail` and keep queue nodes between them to avoid writing lots of `NULL` check conditions.
