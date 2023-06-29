No. of nodes on `i`th level, no. of levels if has total `n` nodes

Max/Min height possible with `n` nodes - min will always be full BT, max will be degenerate tree (equivalent to a LL)

Representations:
- Sequential using arrays (0-indexed): parent at `p`, left child at `2p+1`, right child at `2p+2`
- Recursive using struct/class

Types:
- Full BT: all nodes have 2 children except leaf nodes
- Strict BT: all nodes have either 0 or 2 children
- Complete BT: all nodes have 0 or 2 children and leaf node level has all nodes as left as possible

In a strict binary tree - e = i + 1

Catalan Number (`T(n)`): gives total no. of unique trees possible for `n` nodes. Two forms - using combination and recursive. For labelled (`T(n)*n!`) and unlabelled nodes (`T(n)`).

Traversals:
- preOrder - iterative uses 1 stack
- inOrder - iterative uses 1 stack
- postOrder - iterative uses 2 stack, or 1 stack
- levelOrder - uses a deque
- preOrder, inOrder, postOrder in a single traversal of a tree
