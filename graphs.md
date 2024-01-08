A graph `G(V,E)` is a non-linear data structure comprised of a set of vertices `V` and a set of edges `E`.

## Terminology
- Node/Vertex - labelled or unlabelled
- Edge - directed or undirected, weighted or unweighted

- Path - sequence of adjacent edges connecting nodes
- Degree - `deg(V)`: total number of edges incoming and outgoing from a vertex

- Directed Graph - edges have a direction. Edge `<u, v>` and `<v, u>` are treated as two distinct edges.
- Connected Graph - there is no unreachable vertex. There must be a path between every pair of vertices.
- Finite Graph - number of nodes are finite.

## Properties
- Degree of a graph is twice the number of edges, since each edge connects two nodes: `deg(G) = 2 * E`
- A Tree is nothing but an directed acyclic graph. Linked List and Heaps are also special cases of a graph.

## Representations
Adjacency: two nodes are adjacent only when they are connected by an edge.

- **Adjacency Matrix**: matrix of size `N x N` (`N` is number of nodes). Mark existing edge using `1` in the matrix coordinates. SC = `O(N^2)`.
- **Adjacency List**: for every node, store all its neighbours in a corresponding list. Use an array of vectors - `vector<int> adjList[n]`. SC = `O(2 * E)` for undirected graphs. Much more space efficient than matrix.

For weighted graphs, we can store weight `W` of an edge as `adj[u][v] = W` in adjacency matrix. In adjacency list, use `vector<pair<int, int>> adjList[n]` wehere pair's second element denotes weight of the edge.


## Traversals
- use a visited array with graphs to make sure that already visited nodes aren't visited again, since graphs usually have multiple cycles. This also takes care of traversing any unconnected components.
```cpp
// n nodes, 1-indexed
for(int i = 1; i <= n; i++){
	if(visited[i] == false){
		traversal(i);
	}
}

// inside or before traversal() method, mark visited node as "true"
```

- **BFS**: TC = `O(V + E)` (we visit all vertex and all edges), SC =`O(3 * V)` (queue, visited array, adjList)
```cpp
queue<int> q;
vector<int> bfs; 

// push the initial starting node and mark it as visited
q.push(0);
visited[0] = true;

while(!q.empty){
	int currNode = q.front();
	q.pop();

	// add to traversal ans
	bfs.push_back(currNode);

	// enqueue all its unvisited neighbours and mark all of them as visited
	for(auto it: adjList[currNode]){
		if(visited[it] == false){
			visited[currNode] = true;
			q.push(it);
		}
	}
}

return bfs;
```

**IMPORTANT**: In BFS, mark visited upon enqueue as we may end up with duplicate node entries in the queue if there is a cycle (a neighbour is connected to another of our neighbour). In DFS, mark visited upon actually visiting the node (dequeue/get from stack) as we don't care about (visit) any neighbours for a node and just go to another neighbour instantly.

_Ref_: https://stackoverflow.com/questions/25990706/breadth-first-search-the-timing-of-checking-visitation-status

- **DFS**: TC = `O(V + E)` (we visit all vertex and all edges), SC =`O(3 * V)` (queue, visited array, adjList)
```cpp
void depthFirstSearch(int currNode, vector<int> adjList[], bool visited[], vector<int> &dfs) {
	// mark current as visited and add to traversal ans
	visited[currNode] = true; 
	dfs.push_back(node);
	
	// traverse all its neighbours
	for(auto it : adjList[currNode]) {
	// if the neighbour is not visited, go for it
		if(visited[it] == false){
			depthFirstSearch(it, adjList, visited, dfs);
		}
	}
}
```

_Ref_: https://www.geeksforgeeks.org/why-is-the-complexity-of-both-bfs-and-dfs-ove/

## Techniques
### BFS/DFS and Cycle Detection
**Flood Fill**: in a 2D matrix traverse all neighbours in 4 or 8 directions and traverse entire island (connected component), use a visited 2D matrix of the same size, node will be represented as coordinate `{1, 2}` of type `pair<int, int>`, recolor and enqueue neighbours
**Rotten Oranges**: store all rotten (start points) in queue beforehand, keep level of BFS i.e. `time` info with the coordinate pair `pair<pair<int, int>, int>`, in BFS simulation enqueue - rot and update `time + 1`
**Nearest 0/1 in Matrix**: keep `visArr[][]`, `distArr[][]` to avoid modifications to given matrix. To find dist from `0`, mark all starting points (`0`) as visited and do BFS from all of them keeping `step` info inside a node and copying that to distArr. Note that in for loop of BFS, enqueue (going to a neighbour) and step increment is only for unvisited `1`s (as all `0`s were marked visited beforehand).

- **INSIGHT**: BFS progresses for all starting points in order one level at a time! All starting points (level 0) are executed first (FIFO) and then their respective neighbours (level 1) are added to queue. Therefore we can't have cases where one point traverses lots of steps (say 4) and store it in corresponding `dist[][]` cell while a `0` cell is its immediate neighbour in the above problem.

- **NOTE**: its often not required to keep a `visited[][]` array in the problems above as we can directly modify the given matrix and keep track of modification (visited) state (using color or freshness values). As a rule of thumb, always keep a visited array and avoid mutating the given matrix as it will lead to unnecessary confusion. Ex - case where `initColor = newColor = 0`, we won't know if we've recolored a cell or not since in both the cases its `0` and we'll get TLE.

**Surrounded Regions**: to find `0` surrounded by `1`, do BFS or DFS (desc below) and any univisited `0`s after all traversals are surrounded regions
	- in BFS, put all `0`s on any of the four boundaries in quque and mark them as visited, then do BFS
 	- in DFS, do DFS for all `0`s on any of the four boundaries

**Detect Cycle in Undirected Graph**:
- BFS: keep parent info in node, if node is already visited and its not our parent
- DFS: send current node as parent on dfs call, and we also need to propagate `bool dfs()` call's return value till start of the call stack if and when we find a cycle

A graph is bipartite if the nodes can be partitioned into two disjoint sets `A` and `B` such that every edge in the graph connects a node in set `A` and a node in set `B`. Apart from the mathematical definition, to verify if a graph is bipartite, standard way is to check if it is "2-colorable" (chromatic number = 2) as described below.

**Check Bipartite** - graph can only not be bipartite if it has a cycle, non-cycle graphs are always bipartite. If we can color nodes alternatingly and color all nodes successfully, then it is bipartite. Approach - do dfs/bfs traversal and color nodes alternatingly, on finding an already visited node check its color, if its alternate, then fine, otherwise not bipartite. No need to check for parent here like cycle detection since parent is expected to have alt color and its fine.

Detecting cycle in directed graph:
- DFS - use `pathVis[]` array
- BFS - Kahn's Algorithm (topo sort application)

**Detect Cycle in a Directed Graph (DFS)**: keep a `pathVis[]` array too along with `vis[]` array, mark both on dfs traversal
- upon returning after completing a dfs call for all neighbours of a node `i`, reset `pathVis[i]`, but keep it marked as visited in `vis[]`
- there is a cycle `if(vis[i] == true && pathVis[i] == true)`, else its fine

`pathVis[]` array indicates node's visit status only for the current path we're traversing, therefore upon completing traversal for a node we reset it

**Eventual Safe States (DFS)**: any node is safe if all paths from it lead to a terminal node (i.e node with outdegree = 0)
- all terminal nodes are safe (trivial)
- all nodes whose path till a terminal doesn't involve a cycle are safe (otherwise we end up traversing circularly in cycle - not safe)

Mark it unsafe and propagate if dfs call detects a cycle (all nodes linking to unsafe nodes are marked unsafe with dfs call stack), else we can mark nodes as safe upon return from dfs method call (after all neighbours are processed - for loop).

- **TIP**: Upon returning from dfs method call (after all neighbours are processed) we reset `pathVis[i]=0` but what if a cycle is detected? we just return from the for loop and don't reset pathVis. At the end, `pathVis[i]=0` are safe, others aren't. No need for a `check[]` array this way.

### Topological Sort
Topo sort - linear ordering of vertices such that for every directed edge `u-v`, vertex `u` comes before `v` in the ordering.
- possible only on a DAG
 - multiple such ordering are possible for a given DAG

Techniques to calc topo sort:
- with DFS + stack (can't detect cycles, input must be a DAG)
 - with BFS (Kahn's Algorithm) (works on directed cyclic graphs too)

Applications:
- detect cycle in directed graph using BFS
- course scheduling (printing topo sort)

**Kahn's Algorithm** `O(V + E)`: can detect if topo sort is possible or not, if cycle is present topo sort size isn't equal to total nodes
```txt
Step-1: calc indegree of all nodes and store in indeg[n] array
Step-2: enqueue all indeg[i] == 0
Step-3: do BFS and upon visit put in topo array,
	for neighbours, reduce indegree of neighbour (simulating removal of currNode from graph since we put it in topo)
	and if indegree of neighbour becomes 0 enqueue it
Step-4: compare topo array size == n, if not equal no sorting is possible
```

- **NOTE**: for all problems involving `BFS on Directed Graph`, Kahn's Algorithm is used. Ex - detect cycle in directed graph, topo sort with BFS.

![](https://i.imgur.com/xiZiGeg.png)
