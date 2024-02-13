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
**Next Greater Element**: we are creating a monotonic stack here and keep the top as NGE at all times, also the top element will always be the smalllest in the stack. Traverse the array from the right, and if top is greater than current, top is NGE, else pop out elments till we reach NGE (or empty) in the stack (because those elements can never be NGE since we have a bigger number now (current) being pushed into the stack). Popping elements will make sure that current is the smallest in the stack, on pushing it is the top (new NGE) and stack remains monotonic. Keep storing NGEs in a `res`/`ans` array for querying on later.

Brute Force Quadratic Way (suitable if we have only one element `n` to find NGE of, in that case its linear): we can traverse from the back and keep track of the latest greater element `if(nums[i] > n) ans = nums[i]`, if we reach `target` in `nums[]` we break and output `ans`. For multiple queries, do this for each element in `nums1[]` and replace in `nums1[]` itself to save space.

Apply to circular array (**NGE-II**) - do the same and find NGE from i = 2n-1 to 0 and use `i % n` to access elements, put in another `ans` array only when `i < n`

Smart way to code the same - run while loop for pops first, if stack is not empty we have our NGE otherwise `-1` is the NGE, and push element onto the stack in both cases
```cpp
// cleaner monotonic stack template - NGE

for (int i = nums2.size() - 1; i >= 0; i--) {
  while(!st.empty() && st.top() < nums2[i]) st.pop();

  if(st.empty()) ans[i] = -1;
  else ans[i] = st.top();

  st.push(nums2[i]);
}
```

NGE direct application problem - [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)

**Trapping Rain Water**:
  - water at i = minimum of (maximum at left and right) - level of i  --  time = `O(n^2)`
  - we can precompute and store leftMax and rightMax in two separate arrays -- time = `O(n)`, space = `O(2n)`
  - two pointer approach (optimal):
    - increment left pointer only when there is a greater element on the right pointer, and track `leftMax`, this way we can find water stored at `i` only using `leftMax` since we've reached `i` only because there is a greater element on the right and we don't even need it to calc water stored at `i` as `ans += leftMax - arr[i]`. Do the same for right pointer. Not that when we're on a new `leftMax` there is no water accumulated so just update leftMax, no need to calc water level.
    - how are we sure `leftMax > arr[high]` so much so that we're calc water level without using `arr[high]`? we reached this place `low` because all leftwards elements are smaller than `arr[high]`, that includes `leftMax` ofc (property of two-pointer approach that keeps array sorted in this way)

**Asteroid Collision**: simulation in stack
Cases:
```txt
first asteroid is stable (trivial)

for the nest asteroids, keep comparing and simulating:

both moving in opposite direction (opposite signs)
  - they never collide (going away from each other) - add asteroid to stack
  - collision (moving towards each other)
    - collision and a bigger asteroid is incoming (destory stack top but stay at current asteroid in array; for further collisions)
    - collision and both are equal - both get obliterated (remove from stack, ignore asteroid from array)
    - collision and smaller asteroid is incoming - ignore asteroid from array (do nothing)

both moving in the same direction
  - no collision - add asteroid to stack
```

**Remove k digits to make minimum number**: if the number's digits are monotonic, we can decide which to remove. We always prioritize removing digits on the leftwards (most significant ones) but they have to be greater then elemens on the right too (monotonicity). So we traverse from left to right and remove element from stack if an inversion is encountered (and keep removing atmost `k` times till inversions are present for current digit). Answer number will be reverse stack elements only. Edge cases - duplicate adjacent digits, `01200`, increasing sequence `1234`

Intuition - we remove upto k PLEs for elements which have a guaranteed PLEs and since we're scanning from left to right, PLEs occur on left (more significance than current digit) so we can remove it and expect a smaller final number.

Edge cases handling:
```txt
duplicate adjacent digits - treat duplicates as having no PLE, so we don't pop
01200 - bottom elements of the stack will be 0s, so we form ans and strip all the leading 0s from it (do a check if ans becomes "" after stripping, ans will be "0" then)
1234 and 1324 - if traversal of string is done and k isn't 0, pop rest of the elements from stack top
```

**Largest Rectangle in a Histogram**:
- brute (2n scans): scan and find NSE and PSE for every arr[i]
- better (2 scans): find for each index `area = (next_boundary - prev_boundary +  1) * arr[i]`, stacks stores boundaries (`0`/`index of PSE+1` or `n-1`/`index of NSE-1`) so if there is no PSE/NSE consider it `0`/`n-1` and pre-compute store in `leftSmaller[]` and `rightSmaller[]`, and track `maxArea` throughout the traversal to get ans
- optimal (1 scan): on every new lesser element we pop out existing elements from the stack since they can't be anyone's PSE/NSE later on. The observation is that the current "lesser" element is someone's NSE (it is stack top's NSE!) and stack top's PSE is actually the second element beneath stack top (take `i` or `i - st.top() - 1`). This way we can calc area for each on each for future lesser encountered. For ending histogram bars we do perform pop operations for `i == n` calc are for them.

**Sum of Subarray Minimums**: 
- 3 loops, 2 loops solution to acculmuate min from each subarray
- optimal: find all NSE and PSE for an element and store in pre-compute arrays. Then use combinatorics formula `(g1 + 1) * (g2 + 1)` to calculate no. of subarrays that have that particular element as min, where `g1` is elements between PSE and current element _excluding both_, same for `g2`. For elements which don't have NSE, use ending "size" index to calc between elements, so do `arr.size() - i - 1`

```txt
total number of subarray formula derivation:

subarray starting index choices = g1 + 1    :g1 is no. of elements strictly between PSE and curr
subarray ending index choices = g2 + 1      :g2 is no. of elements strictly between NSE and curr

total choices = (g1 + 1) * (g2 + 1)
```

**Sum of Subarray Ranges**: same as above but here we use PGE, NGE arrays too.

**Online Stock Span**: simple problem of monotonic stack, but notice that we pop off smaller previous elements (and count span++ for each) but if a bigger one comes in future then it will cover all previous lesser's span too so we need to store span along with stack element and add it up in future biggers, thus stack element is of the form `stack<pair<int, int>` i.e. `{price, span}`

**The Celebrity Problem**: use elimination technique (if A knows B, discard A as it can't be the celeb but B maybe and vice-versa), do pairwise comparisons to find the celeb. Need an extra step at last to check if remaining element is actually a celeb
- use stack: pop two elements from the stack and compare them, push potential celeb back into the stack. Do this till a single element remains in the stack
- use matrix cell as marker: if `mat[i][i]` is `0` or `1` represents `i` is a celeb of not, start from person `r = 0` and check if he knows person `i` by looping over all its columns (other persons), if `mat[r][i] = 1` make `mat[i][i] = 1` (i is potential celeb) and new `r = i` else make `mat[r][r] = 1`. We won't ever reach the celeb's `mat[i][i]` cell, so only one diagonal element should be `1` and do verification for it as the last step
- two-pointer approach: one at the start one at the end, check if last remaining element `i == j` is a celeb or not

**LRU Cache**: use DLL and `unordered_map<key, Node*>`. Use two dummy nodes `(-1, -1)` as `head` and `tail` and keep queue nodes between them to avoid writing lots of `NULL` check conditions.
