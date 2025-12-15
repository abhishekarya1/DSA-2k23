## Basics
**Some Fundamental formulae**:
| Property  | 0-indexed levels  | 1-indexed levels  | Notes
|---|---|---|---|
| Max number of nodes on i-th level | $`2^i`$ | $`2^{i-1}`$ | |
| Levels in tree with `n` nodes | min = $`\lfloor \log_2 n \rfloor`$ <br> max = $`n - 1`$ | min = $`\lfloor \log_2 n \rfloor + 1`$  <br> max = $`n`$ | min makes a perfect BT <br> max makes a degenerate tree; equiv to a LL |
| Nodes in tree of height `h` | min = $`2^{h+1} - 1`$ <br> max = $`h + 1`$ | min = $`2^h - 1`$ <br> max = $`h`$ | |

Ways to derive the last formula:
- workout a relation between each level's index and total nodes till that level
- number of nodes on each level follow a GP $`2^0 + 2 + 4 + 8 + 16 + ... + 2^n`$ with common ratio `2` so we can find its sum
- in a Perfect BT, the `i`-th level has $`2^i`$ nodes and sum of nodes in all prior levels are $`2^i - 1`$ so we can sum them as $`2^i + 2^i - 1`$ which comes out to be $`2^{i+1} - 1`$ after simplification.

**Representations**:
- **Sequential**: using arrays (0-indexed): parent at index `p`, left child at `2p + 1`, right child at `2p + 2`
- **Recursive**: using `struct` or `class`

**Types of BT**:
- **Perfect**: all nodes have exactly `2` children except leaf nodes, or we can say that all leaf nodes are on the same level
- **Full / Proper / Strict**: all nodes have either `0` or `2` children
- **Complete**: all nodes have `0` or `2` children and leaf node level has all nodes as left as possible

**Leaf–Internal Node Formula**: in Full BT, $`e = i + 1`$ where `e` = leaf nodes (external nodes) , and `i` = non-leaf nodes (internal nodes). In a Perfect BT, this holds true for every level.

**Catalan Number** (`C(n)`): gives total no. of unique trees possible for `n` nodes, some are original and others are mirror images. For un-labelled nodes its `C(n)`, and for labelled nodes its `C(n) * n!`. Written in two forms - using combination or as a recurrence relation. It is used in many diff applications, one of which is to calc the number of unique well-formed parentheses combinations if we're given `n` open and `n` closed (total `2n`) parentheses. ([ref](https://en.wikipedia.org/wiki/Catalan_number))

## Traversals
**Traversal Trick**: reading traversals using finger placement around nodes: `left = pre-order`, `bottom = in-order`, `right = post-order`.

**Pre-Order**: ([link](https://leetcode.com/problems/binary-tree-preorder-traversal/)) iterative uses 1 stack (print root and put right then left in stack - strategic).

**In-Order**: ([link](https://leetcode.com/problems/binary-tree-inorder-traversal/)) iterative uses 1 stack. Go as left as possible until curr node is not `NULL` and keep pushing to stack, after extreme left is reached then print and pop top of stack and go rightwards. Do this `while (curr || !st.empty())` which signifies if there are elements left to process.

**Post-Order**: ([link](https://leetcode.com/problems/binary-tree-postorder-traversal/)) iterative uses 2 stacks (post-order is nearly reverse of pre-order, second stack is for reversal), `st1` is primary and keep popping nodes and then push thier left and right as usual, on each popped node from `st1`, push it to `st2`.
  - **using 1 stack**: if we store `root → right → left` order and then reverse the result, that's post-order.

**Level-Order**: ([link](https://leetcode.com/problems/binary-tree-level-order-traversal/)) use a `queue`, at the start of each level, the size of queue is the number of nodes in that level so use a `for` loop to limit.

**All representations in a single traversal**: `stack<pair<TreeNode*, int>>` while stack is not empty, on visit `1` (add to pre-order list) add back to stack and go left, on visit `2` (add to post-order list) add back to stack, and go right, otherwise (visit `3`; add to in-order list), add back to stack and go nowhere.

> [!TIP]
> Dry run any traversal on tree `1 2 3` to quickly verify.

## Medium Problems

**Height/Depth of a BT**: ([link](https://leetcode.com/problems/maximum-depth-of-binary-tree/)) `return 1 + max(leftHeight, rightHeight)` for a node

In below problems we don't use normal height method (that'll increase recursive method call levels) rather we modify height method to calc:
- **Check Balanced**: ([link](https://leetcode.com/problems/balanced-binary-tree/)) `abs(leftHeight - rightHeight) < 1` for every node, use `-1` to cascade failure in a normal height method
- **Diameter**: ([link](https://leetcode.com/problems/diameter-of-binary-tree/)) `leftHeight + rightHeight` (notice there is no `+ 1` while calc diameter because its path, not nodes), in normal height method track `maxDiameter` for every node
- **Maximum Path Sum**: ([link](https://leetcode.com/problems/binary-tree-maximum-path-sum/)) track max for sum `sum = max(sum, curr -> val + leftSum + rightSum)`, return value of the modified height method will be `curr->val + max(leftSum, rightSum)` (non-curving point nodes)

## Views and Traversals
- Zig-Zag Traversal - use modified level-order traversal. `int idx = leftToRightFlag ? i : (queueSize - 1 - i)`
- Boundary Traversal - leftSide non-leaves, all leaves, rightSide non-leaves
- Vertical Order Traversal - `queue<TreeNode*, pair<int, int>>` to store nodes for level order traversal, `map<int, pair<int, multiset<int>>>`. We can use anyOrder traversal to do it.
- Top View of a BT - store one node per vertical level in `map<int, int>`, don't store if it already exists. Use `queue<pair<int, TreeNode*>>`
- Bottom View of a BT - same as top view but keep replacing with node on the same vertical level
- Left/Right View of a BT - `if(level == ds.size()` and subsequently move to `moveRight` for right view and `moveLeft` for left view. We can use modified level-order traversal too.
















