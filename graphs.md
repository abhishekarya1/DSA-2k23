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

### Techniques
- **Flood Fill**: in a 2D matrix traverse all neighbours in 4 or 8 directions and traverse entire island (connected component), use a visited 2D matrix of the same size, node will be represented as coordinate `{1, 2}` of type `pair<int, int>`, recolor and enqueue neighbours
- **Rotten Oranges**: store all rotten (start points) in queue beforehand, keep level of BFS i.e. `time` info with the coordinate pair `pair<pair<int, int>, int>`, in BFS simulation enqueue - rot and update `time + 1`
- **Detect Cycle** - keep parent info in node, and use:
	- BFS: if node is already visited and its not our parent
 	- DFS: same as above, and we also need to propagate till start of the call stack if we find a cycle   
- **Nearest 0/1 in Matrix**: keep `visArr[][]`, `distArr[][]` to avoid modifications to given matrix. To find dist from `0`, mark all starting points (`0`) as visited and do BFS from all of them keeping `step` info inside a node and copying that to distArr. Note that in for loop of BFS, enqueue (going to a neighbour) and step increment is only for unvisited `1`s (as all `0`s were marked visited beforehand).

**NOTE**: its often not required to keep a `visited[][]` array in the problems above as we can directly modify the given matrix and keep track of modification (visited) state (using color or freshness values). As a rule of thumb, always keep a visited array and avoid mutating the given matrix as it will lead to unnecessary confusion. Ex - case where `initColor = newColor = 0`, we won't know if we've recolored a cell or not since in both the cases its `0` and we'll get TLE.
