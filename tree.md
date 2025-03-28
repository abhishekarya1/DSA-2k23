No. of nodes on `i`th level, no. of levels if tree has total `n` nodes - work out formulas with `log2` and `2^n` based on indexing of root is 0 or 1

Observation - No. of nodes on each level follow GP - `1 + 2 + 4 + 8 + 16 ...` with common ratio 2

Max/Min height possible with `n` nodes - min will always be full BT, max will be degenerate tree (equivalent to a LL)

Representations:
- Sequential using arrays (0-indexed): parent at `p`, left child at `2p+1`, right child at `2p+2`
- Recursive using struct/class

Types:
- Full BT: all nodes have exactly 2 children except leaf nodes
- Strict BT: all nodes have either 0 or 2 children
- Complete BT: all nodes have 0 or 2 children and leaf node level has all nodes as left as possible

In a strict binary tree - e = i + 1 where e = leaf nodes, and i = non-leaf nodes

Catalan Number (`T(n)`): gives total no. of unique trees possible for `n` nodes, some are original and others are mirror images. For labelled (`T(n)*n!`) and unlabelled nodes (`T(n)`). Written in two forms - using combination or recursive.

Reading traversals using finger placement around nodes - left = preOrder, bottom = inOrder, right = postOrder

### Traversals
- preOrder - iterative uses 1 stack (print root and put right then left in stack - strategic)
- inOrder - iterative uses 1 stack (go as left as possible for non NULL nodes, for NULL nodes (leaf) print top of stack and go rightwards) - `while(true)` way is used here and traversal only by using stack top is not possible here unlike preOrder and postOrder's `while` loop
- postOrder - iterative uses 2 stack (postOrder is nearly reverse of preOrder, second stack is for reversal) (put root in stack2, then push left then right in stack1)
  - using 1 stack - similar to inOrder traversal but requires another `while` loop inside logic to print root(s)
- levelOrder - uses a queue, put a `for` loop inside to track level
- preOrder, inOrder, postOrder in a single traversal of a tree - `stack<pair<TreeNode*, int>>` while stack is not empty, on visit 1 (add to preOrder list) and go left, on visit 2 (add to postOrder list and go right), otherwise (visit 3) add to inOrder list and go nowhere

### Medium Problems
- Height of a BT: `return 1 + max(leftHeight, rightHeight)` for root

In below problems we don't use normal height method (that'll increase recursive method call levels) rather we modify height method to calc:
- isBalanced: `abs(leftHeight - rightHeight) < 1` for every node, use `-1` to cascade failure in a normal height method
- Diameter: `leftHeight + rightHeight` (notice there is no `+ 1` while calc diameter because its path, not nodes), in normal height method track `maxDiameter` for every node
- Maximum Path Sum: track max for sum `sum = max(sum, curr -> val + leftSum + rightSum)`, return value of the modified height method will be `curr->val + max(leftSum, rightSum)` (non-curving point nodes)

### Views and Traversals
- Zig-Zag Traversal - use modified level-order traversal. `int idx = leftToRightFlag ? i : (queueSize - 1 - i)`
- Boundary Traversal - leftSide non-leaves, all leaves, rightSide non-leaves
- Vertical Order Traversal - `queue<TreeNode*, pair<int, int>>` to store nodes for level order traversal, `map<int, pair<int, multiset<int>>>`. We can use anyOrder traversal to do it.
- Top View of a BT - store one node per vertical level in `map<int, int>`, don't store if it already exists. Use `queue<pair<int, TreeNode*>>`
- Bottom View of a BT - same as top view but keep replacing with node on the same vertical level
- Left/Right View of a BT - `if(level == ds.size()` and subsequently move to `moveRight` for right view and `moveLeft` for left view. We can use modified level-order traversal too.
