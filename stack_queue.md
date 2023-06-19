### Stack and Recursion
- Sort stack using recursion
- Reverse stack using recursion

### ADT
- Stack using:
  - Arrays: insertion and deletion at end using ` int top;` index
  - Linked List: insertion and deletion at head
- Queue using: 
  - Arrays: use circular inserts to avoid wastage of space. Maintain a `currSize` and do `(rear+1)%arrSize; currSize++` on every push to queue
  - Linked List: insertion at end, deletion at head

- Stack using Queue: bring recently added element to front by extracting and pushing front to rear `n-1` times
- Queue using Stack: have to use 2 stacks. Either make push `O(1)` and pop `O(n)` or vice-versa.


### Classic Problems
**Check Balanced Parentheses**: push open brackets, pop if stack top matches current. Invalid cases:
  - when brackets at stack top and current don't match
  - if we encounter a closing bracket but stack is empty
  - when we've reached till the end and stack is still not empty
  
**Min Stack**: retrieves minimum element in `O(1)` time. If we have a new minimum push `2*element - min` into the stack, it is guaranteed that this value we are pushing is less than element we are pushing (also our new minimum) and this position is where we encountered a new minimum. While popping, we need to make sure that our minimum changes when we are popping the minimum element from the stack so we check for change point and then modify minimum accordingly. [link](https://www.baeldung.com/cs/stack-constant-time)

### Prefix, Infix, Postfix
**Postfix to ANY**: scan from left to right, if operand then push onto to stack, if operator then pop 2 elements from stack and form infix/postfix for them and push them back to the stack, at the end there will only be one element in the stack and that is our answer

**Prefix to ANY**: scan from _right to left_ and do the same as above

**Infix to Postfix**: if operand then print it, if operator and its precedence is greater than stack top (perform it now i.e. print it), otherwise push it to stack, at the end print all stack elements if expression traversal is finished

**Infix to Prefix**: reverse prefix expression, we're making it "nearly-postfix" by converting `(` to `)` and vice-versa, convert this using `infixToPostfix()` and store in `resultString`, reverse `resultString` again

### Monotonic Stack
