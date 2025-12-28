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

## 3 Core Traversals
**Traversal Trick**: reading traversals using finger placement around nodes: `left = pre-order`, `bottom = in-order`, `right = post-order`.

**Pre-Order**: ([link](https://leetcode.com/problems/binary-tree-preorder-traversal/)) iterative uses 1 stack (print root and put right then left in stack - strategic).

**In-Order**: ([link](https://leetcode.com/problems/binary-tree-inorder-traversal/)) iterative uses 1 stack. Go as left as possible until curr node is not `NULL` and keep pushing to stack, after extreme left is reached then print and pop top of stack and go rightwards. Do this `while (curr || !st.empty())` which signifies if there are elements left to process.

**Post-Order**: ([link](https://leetcode.com/problems/binary-tree-postorder-traversal/)) iterative uses 2 stacks (post-order is nearly reverse of pre-order, second stack is for reversal), `st1` is primary and keep popping nodes and then push thier left and right as usual, on each popped node from `st1`, push it to `st2`.
  - **using 1 stack**: if we store `root → right → left` order and then reverse the result, that's post-order.

**Level-Order**: ([link](https://leetcode.com/problems/binary-tree-level-order-traversal/)) use a `queue`, at the start of each level, the size of queue is the number of nodes in that level so use a `for` loop to limit.

**All representations in a single traversal**: `stack<pair<TreeNode*, int>>` while stack is not empty, on visit `1` (add to pre-order list) add back to stack and go left, on visit `2` (add to post-order list) add back to stack, and go right, otherwise (visit `3`; add to in-order list), add back to stack and go nowhere.

> [!TIP]
> Dry run any traversal on tree `1 2 3` to quickly verify.

## Modified Height Method

**Height/Depth of a BT**: ([link](https://leetcode.com/problems/maximum-depth-of-binary-tree/)) return `1 + max(leftHeight, rightHeight)` for every node, for `NULL` node return `0`.

In below problems we don't use the normal height method, rather we modify height method's body, prune based on a specific condition, or tweak the returned value acc to problem.

**Check Balanced Tree**: ([link](https://leetcode.com/problems/balanced-binary-tree/)) on any node if `abs(leftHeight - rightHeight) > 1` then `return -1`, use `-1` to prune and cascade failure in the usual height method.

**Diameter of BT**: ([link](https://leetcode.com/problems/diameter-of-binary-tree/)) treat each node as a curving-point and calc diameter for it as `leftHeight + rightHeight` (notice there is no `+ 1` while calc diameter because its path, not nodes), track `maxDiameter` for every node.

**Maximum Path Sum**: ([link](https://leetcode.com/problems/binary-tree-maximum-path-sum/)) use Kadane's algorithm, track `max(sum, curr -> val + leftSum + rightSum)`, return value of the modified height method is `curr->val + max(leftSum, rightSum)` i.e. consider only the path with max sum starting from current node (curving-point). Basically we are only sending upwards the max positive sum path to be considered for a node for contributing to path sum of a node upwards in the ancestry.

## Specific Traversals and Views

**Zig-Zag Traversal**: ([link](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)) use modified level-order traversal, put elements on index as `idx = leftToRightFlag ? i : (queueSize - 1 - i)`.

**Boundary Traversal**: ([link](https://takeuforward.org/data-structure/boundary-traversal-of-a-binary-tree)) three parts by calling 3 functions from the `root` node. Left boundary non-leaves (`addLeftBoundary` function), then all leaves (`addLeaves` function), and finally right boundary non-leaves but in reverse order (`addRightBoundary` function).

**Vertical Order Traversal**: ([link](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/)) `queue<TreeNode*, pair<int, int>>` to store nodes for level order traversal, ordered `map<int, map<int, multiset<int>>>` for tracking co-ordinates. We can use any of the 3 core traversals or even level order traversal to solve this. The `multiset<int>` is used to deal with overlapping nodes (`2` nodes on the exact same co-ordinate), otherwise we could've used `vector<int>` too.

**Top View**: store one node per vertical level in `map<int, int>`, don't store if a level's node already exists. Use `queue<pair<int, TreeNode*>>`. Can't use 3 core traversals as they may goto level `+1` in left subtree itself first (this limitation can be bypassed by tracking level too), use level-order only for simpler code.

**Bottom View**: exactly the same as top view, but just keep replacing map entry with every new node we encounter on the same vertical level i.e. last node of a vertical level is left in map when we end the entire traversal.

**Left / Right View**: ([link](https://leetcode.com/problems/binary-tree-right-side-view/)) smarter way by checking `if(level == ds.size())` and store current node in answer list if `true`, subsequently move to `right` for right view and `left` for left view. We can use any traversal, but using 3 core ones make the code much simpler than level order.

## Paths

**Root to Node Path**: DFS + Backtracking, "go and check" way of writing code is simpler to understand and cleaner as usual.

**Lowest Common Ancestor (LCA)**: ([link](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)) only the LCA node will have target nodes in both its left and right subtree, rest will have them only in one subtree - either left or right. From each node, goto both subtrees and return `NULL`, `p`, or `q` when we encounter them otherwise goto return value from left subtree if right subtree return value is `NULL`, or return value from right subtree if left subtree return value is `NULL`, or return current node (LCA found) if return value from both subtrees are not `NULL`.

**Maximum Width of BT**: ([link](https://leetcode.com/problems/maximum-width-of-binary-tree/) use BFS and consider index of nodes as `2*curr + 1` for left and `2*curr + 2` for right. For each level, track min present index and max present index, and calc and track max width as `right - left + 1`.
