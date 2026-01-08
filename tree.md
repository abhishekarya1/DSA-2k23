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

**Left / Right View**: ([link](https://leetcode.com/problems/binary-tree-right-side-view/)) smarter way by checking `if(level == ds.size())` and store current node in answer list if `true`, subsequently move to `right` for right view and `left` for left view, track only horizontal levels for this. We can use any traversal, but using 3 core ones make the code much simpler than level order.

## Paths

**Root to Node Path**: DFS + Backtracking, "go and check" way of writing code is simpler to understand and cleaner as usual.

**Lowest Common Ancestor (LCA)**: ([link](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)) only the LCA node will have target nodes in both its left and right subtree, rest will have them only in one subtree - either left or right. From each node, goto both subtrees and return `NULL`, `p`, or `q` when we encounter them otherwise goto return value from left subtree if right subtree return value is `NULL`, or return value from right subtree if left subtree return value is `NULL`, or return current node (LCA found) if return value from both subtrees are not `NULL`.

**Maximum Width of BT**: ([link](https://leetcode.com/problems/maximum-width-of-binary-tree/)) use BFS and track index of nodes in queue as `2*curr + 1` for left and `2*curr + 2` for right. For each level, check min present index `i = 0` and max present index (`i = n-1`), and calc and track max width as `right - left + 1`.

**Satisfy Children Sum Property** (with only addition allowed): go downwards and if children sum is greater, then make it current root's val otherwise if children sum is lesser then make either (or both) children val as current root's. This is to ensure that when we come back moving upwards, then sum of children will definitely be greater than their root. Basically, we're propagating bigger values towards the root of the tree (upper portion) towards lower portion because if its vice-versa then the tree could never satisfy the property i.e. lower nodes sum will always be lesser than their respective roots.

**Count nodes in Complete BT**: for every subtree, if left extreme height and right extreme height are equal then its a full BT (use formula `2^h -1` to count nodes and return it), otherwise recurse and return `1 + lCnt + rCnt`.

## Construction
Inorder traversal is mandatory for unique construction unless the tree is guaranteed to be full. If full BT, then preorder + postorder can do as child nodes will always occur in pair (or not exist at all). Pre/post order gives the root order and inorder splits left/right subtrees, and can find out the tree by doing these two steps recursively. ([link](https://takeuforward.org/data-structure/requirements-needed-to-construct-a-unique-binary-tree-theory))

**Construct using Preorder + Inorder**: ([link](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)) recursively written in a very smart and intuitive way (just like its done with pen and paper).

**Construct using Postorder + Inorder**: ([link](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)) use reverse postorder (`root -> right -> left`) and tweak things a little, rest all is same as above. Alt more cleaner way is to pick `postEnd` always and process bounds from right to left in `postorder` array.

**Serializing / Deserializing BT**: ([link](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)) serialization is just comma-separated BFS order traversal string with `#` in place on `NULL` nodes (do BFS and push `NULL` children of a node too to queue). Deserialization is similar using a queue - init by putting deserialized root node in queue, take out a node from queue and attach to it its left and right child (non `#` only) and push back child in queue, for `#` attach `nullptr` as child but don't push back into queue, use `stringstream` and `getline` with delimiter to split access string. Obsv - in deserialization, once a node is processed and its children's corresponding string index accessed in serialized string, we never return to any of them so we can just keep moving forward in serialized string during deserialization and parent-child indexing takes care of itself.

**Morris Traversal** (Inorder, Preorder): TC = `O(n)`, SC = `O(1)`, no recursion or auxiliary space. Uses Threaded BT with 3 cases based on left subtree's existence and righmost node in it.

**Flatten BT to LL**: ([link](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)) we convert it to a right-skewed tree (LL), do `right -> left -> root` traversal and flatten recursively (or iteratively using stack) by attaching nodes as follows: current root's `right` becomes the `prev` node and `left` becomes `NULL`. `prev` can be tracked globally in recursion and in iterative stack solution, the top node in stack after we pop current root out of it will be the `prev` node (as we put `right` then `left` into the stack). We can also use Morris traversal (no extra space). 

## Binary Search Tree (BST)
> A type of BT in which all nodes in the left subtree are smaller than the parent, and all nodes in the right subtree are larger, making it efficient for searching, inserting, and deleting data by maintaining elements in a sorted order.

- `left < root < right`, if we've duplicates then we need to define where they go - either right or left. We can also store freq count in the node and keep BST nodes unique.
- operations especially search is `O(logn)` unlike BT's `O(n)`. Since searching in BST doesn't need to goto all nodes, we just traverse a single path based on conditions and we'll find our target if its there.
- inorder traversal gives the sorted order `[1,2,3,4,5,6,7]` and reverse-sorted order can be found by doing reverse inorder traversal (right, root, left).
- constructing a BST with solely the preorder traversal is possible.

**Search a Target Value**: ([link](https://leetcode.com/problems/search-in-a-binary-search-tree/)) like Binary Search, tail recursion returning a `TreeNode*`.

**Min / Max**: ([link](https://www.geeksforgeeks.org/dsa/find-the-minimum-element-in-a-binary-search-tree/)) the leftmost node while traversing straight from root will be min, and rightmost will be max. It is not possible for a node to be not in leftmost straight path and be lesser than its respective root in that path because all subtree nodes must also follow property in a BST.

**Floor / Ceil**: traverse down iteratively and track a `prev` node. At the end, `prev` node will be the ans.

**Insert a Node**: ([link](https://leetcode.com/problems/insert-into-a-binary-search-tree/)) iterative way is very simple, just traverse a path and insert at last leaf by tracking `prev` node. Recursive is more terse code, works by traversing path using recursion and if-else and inserts the new node using function return. Summary - only one call creates the node; all other returns exist to rebuild the path back to the original root correctly.

**Delete a Node**: ([link](https://leetcode.com/problems/delete-node-in-a-bst/)) recursion code is similar to the insertion code above, find the inorder successor (smallest node in right subtree), replace the current node's data with the successor's, and recursively delete the successor from the right subtree. This is effectively just moving elements leftwards erasing the element we want to delete in inorder traversal of BST e.g. `[1,2,3,4,5,6,7]` with root `4`.

**`K`th Smallest / Largest Node**: ([link](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)) use BST property that inorder traversal gives sorted order of nodes, just return node value on the `k`th node. Use reverse-inorder to traverse in decreasing order and return the `k`th node's value as its the largest.

**Validate BST**: ([link](https://leetcode.com/problems/validate-binary-search-tree/)) given a BT, check if its BST. We'll need to check all nodes in left subtree are lesser than a root, and vice-versa. Use a range starting with `[LONG_MIN, LONG_MAX]` and keep checking if a root's val is within its range, reduce range when moving left or right accordingly.

**LCA in BST**: ([link](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)) use BST property that if current node is lesser than both target nodes then go right, else if its greater than both then go left, otherwise if it lies in middle of bothm then it is the LCA node.

**Construct BST from Preorder**: ([link](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/))
- brute force approach where we attach every node from preorder to tree starting from root, `O(n^2)`
- sort preorder and get inorder and do as we used to construct BT, `O(nlogn)`
- use upper-bound and attach nodes acc, `O(n)`. Every root has lesser nodes on its left (recursively too) in preorder array and we'll keep attaching those to its left until we can and keep updating upper-bound as they may also recursively have children, we'll then attach `NULL` when no more left portion remains, and then go right but upper-bound remains the same because we're still under same root finding its right portion in preorder array.
