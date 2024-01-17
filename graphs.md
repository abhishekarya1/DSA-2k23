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
- **Adjacency List**: for every node, store all its neighbours in a corresponding list and worst case there can be `V` unconnected components. Use an array of vectors - `vector<int> adjList[n]`. SC = `O(2 * E + V)` for undirected graphs. Much more space efficient than matrix.

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

First technique yet that doesn't involve a visited array `vis[]`, it isn't required since we're enqueue only `indeg = 0` nodes as soon as they become one.

```cpp
// enqueue indeg[i] == 0 nodes

while(!q.empty()){
	int node = q.front();
	q.pop();

	// notice we add to topo sort here in BFS!
	topo.push_back(node);		

	for(int e : adjList[node]){
		indeg[e]--;
		if(indeg[e] == 0) q.push(e);
	}
}

// check topo.size() == n here  
```

- **NOTE**: for all problems involving `BFS on Directed Graph`, Kahn's Algorithm Topo Sort is used. Ex - detect cycle in directed graph, find topo sort, eventual safe state.

![](https://i.imgur.com/xiZiGeg.png)

**Eventual Safe States (BFS)** - topo sort using Kahn's algo stops just before a cycle starts, we can use this phenomena but Kahn's algo works on indegrees and terminal nodes can only be identified with their outdegree. Reverse every edge in the graph and then terminal nodes will have indegree as 0, we can then do Kahn's for these terminal nodes and whichever node we can reach will be safe (normal topo sort traversal). We will be stopped just before a cycle and those nodes won't be part of topo sort list and won't be safe either so it works out.

### Shortest Path
Relaxing the edges - populate `dist[]` using minimum distance logic shown below:
```cpp
if(dist[node] + wt < dist[neighbour]){
	dist[neighbour] = dist[node] + wt;
	q.push({dist[neighbour], neighbour});	// for Dijkstra's algorithm (PQ enqueue)
}
```
We're always given a source node in these problems as cost to goto source node is always zero (`0`).

**Shortest path in DAG**: find topo sort using `DFS + stack` and populate `dist[]` using minimum distance logic for topo sort nodes in order (keep popping stack and process)

Unconnected components? keep popping till we reach source node. Distance to travel to unconnected components will remain infinity (`INT_MAX`).

**Shortest path in undirected graph with unit weight**: do a simple BFS and populate `dist[]` using minimum distance logic, we can reduce code by just keeping `<node, steps>` as a node and `vis[]` since the first time we visit a node, that'll be the shortest distance if all edges have unit (`1`) weight, so its just a normal BFS traversal with step increments like 0/1 matrix problem.

**Shortest path using Dijkstra's Algorithm**: 
- greedy approach based on BFS, we choose miniumum edge weight among all the neighbours and visit it first
- TC = `O(E * log V)`
- doesn't work on graphs that have a negative weight
- use min-heap (`priority_queue`) with nodes as `<dist, node>`

`priority_queue` gives us minimum distance neighbour for each node _early on_, with `queue` we can achieve the same output but we'll traverse for all nodes and writing to `dist[]` only if a lesser distance is found for a neighbour (leading to more TC).

```cpp
int src = 0;
pq.push({0, src});
dist[src] = 0;

while(!pq.empty()){
	auto node = pq.top();
	pq.pop();
	int currNode = node.second;
	int costSoFar = node.first;

        // early exit (optional)
        if(currNode == dst) return dist[dst];

	// check all neighbours
	for(auto e : graph[currNode]){
		int nNode = e[0];
                int edgeWt = e[1];

                // relax edge - calc new cost and update if apt
                if(costSoFar + edgeWt < dist[nNode]){
                    dist[nNode] = costSoFar + edgeWt;
                    pq.push({costSoFar + edgeWt, nNode});
                }
	}
}

return dist[dst];
```

TC analysis of Dijkstra's algorithm:
```txt
V * (log(heap_size) + E * log(heap_size))
V * (log(hs) + (V - 1) * log(hs))		:since every node can have max V-1 neighbours (edges)
V * V * (log(hs)
V * V * log(V * V)		:since each node can at max put all its neighbours in PQ - (V*(V-1)) ~ V*V (full mesh graph)
V * V * 2 * log(V)
E * log(V)		:since V*V is total edges (E) (full mesh graph)
```

- **INSIGHT**: visited array `vis[]` isn't required in shortest distance problems as when we visit a node (say with cost `2`), to come back we will require another `2` cost and that `4` weight isn't lesser than previous cost `0` if we started at source node.

**Shortest Path in a Binary Maze (0/1 Grid)**: we don't really need to use Dikstra's here since for each node all edges of the graph are equal (unit weight). Use normal Q instead of PQ and update `dist[][]`. This problem is identical to "Shortest path in undirected graph with unit weight" described above, but on 2D grid so `m * n` nodes with each `node = <row, col>`. The first time we visit a node, that'll be its minmum distance so we can ditch either the dist[] array (use vis[] instead) or steps info in node since they will always be the same value.

LEARNING: All edges of graph having the same weight is equivalent to undiercted graph and we can use simple BFS to find shortest path in it. Edge relax logic can also be written either way in such equal weight edges: `dist[currNode] + cost < dist[neighbourNode]` instead of Dijkstra's standard current path checking `currNode.costSoFar + cost < dist[neighbourNode]`.

**Path with Minimum Effort**: Track max of current path inside the queue node `node = <effortSoFar, <row, col>>`, and on every new max `newEffort > effortSoFar` encountered, update dist array and enqueue neighbour node if its a new min for a neighbour node `heights[nRow][nCol]` on `newEffort < dist[nRow][nCol]`. [My LC solution](https://leetcode.com/problems/path-with-minimum-effort/solutions/4553588/simple-dijkstra-s-using-min-heap-approach-c-concise-well-commented-self-explanatory/)

LEARNING: Early Exit in Dijkstra is possible unlike simple BFS shortest path - upon dequeue of `dst` node we can break and return `dist[dst]` as in Dikstra we never change `dist[]` of (ergo enqueue) an already popped off node till the end. This is because we reached that node with the minimum distance possible greedily and whatever nodes are remaining in the queue even if they touch dst node again to modify distance it will be more since it was min-heap and we already picked min. [Ref](https://stackoverflow.com/questions/23906530/dijkstras-end-condition) 

Contrary to the above, its possible in shortest path simple BFS to dequeue a node twice since it can be enqueued again with a shorter distance as in example below:

![](https://i.imgur.com/pLkKlwO.png)

Often we use Q with Dijkstra (becomes simple BFS then) and save on that extra `log(V)` TC of heap insertion and deletion. But when do we NOT use PQ in Dijsktra (simple BFS suffices)?
- When we notice that parameter we're using to minimize with min-heap `node = {parameter, {row, col}}` (can be cost or steps) is always increasing by a **fixed amount** for every neighbour (like unit weights) then there is no point in choosing minimum for going to a neighbour as they're all equal, we're better off using a normal queue since all nodes can be inserted in queue in any order (FIFO or Min-first).

Remember Dijkstra is just optimized BFS so doing BFS when we don't have monotonically increasing edges weights, we'll do worse TC with a Q in that case - `O(E*V)` as we'll encounter min multiple times and then enqueue/dequeue it accordingly everytime. But in Dijkstra, we never change min dist of an already dequeued node, all previously enqueued copies of it don't lead to relaxation as we already wrote min to dist array.

**Cheapest Flights Within K Stops**: build a graph first, and then enqueue src `node = {stops, {currNode, costSoFar}}`, cap `stops <= k`, and we can use Q here instead of PQ since parameter i.e. `stops` always increments by `1` when visiting a neighbour - so simple BFS. Can't take any early exit since dest node can be visited **again** via another path within `k` with lesser cost so `return dist[dest]` after everything is done.

LIMITATION of Dijkstra: Why didn't we use Dijkstra with `node = {costSoFar, {currNode, stops}}`? Since we're minimizing on distance, whenever we visit intermediate nodes (non-dest) we try to store minimum distance for it but optimal path may not be miminum till that point, we want overall distance till `dest` node as shortest but optimizing distance for intermediate path nodes can be wrongful as we can exhaust `k` paths trying to chase mins on every neighbour and not reach `dest` at all. And alternate routes with more weight (but maybe lesser than `k` stops) leading to those intermediate nodes won't be considered (enqueued) and we won't traverse such paths. We take parameter as `k` and try to minimize distance within it.

**Number Of Ways To Reach End With Shortest Distance**: use Dijkstra with `ways[]` array with `ways[src] = 1`, on finding a new min while doing edge relaxation `ways[nNode] = ways[curNode]`, and on finding equal edge `ways[nNode] = ways[nNode] + ways[curNode]`. This works out because on a new min we replace `ways[nNode]` and then increment from there.

**Bellman-Ford Algorithm**: 
- can work on negative edge weights and even negative cycles (no TLE here)
- can work directly on unordered edges without even building a graph first
- all edges gets relaxed in `V - 1` iterations, if `V`th iteration still relaxes an edge, this means there is a negative cycle
- TC = `O(V * E)` relaxing `E` edges in `V - 1` iterations
- worst case is a linked list where relaxation only happens for last edge of each iteration
- to use for undirected graph, we make two diff direction directed edges between two nodes with same weight

The Bellman-Ford algorithm requires `N-1` iterations to guarantee that all the shortest paths from the source to all other vertices are found. This is because the longest possible shortest path in a graph with `N` vertices is of length `N-1`.

**Floyd–Warshall Algorithm**: calc shortest path from every node to every node, via every node. It is brute-force approach that has cubic TC.
- can work on negative edge weights and even negative cycles (no TLE here)
- AM is modified in-place or a new Cost Matrix is made that contains every node to every node minimum distance
- go from node `i` to node `j` via node `k`, do `mat[i][j] = min(mat[i][j], mat[i][k] + mat[k][j])`, and keep lesser distance
- at any point if it is relaxed such that `mat[i][i] < 0`, this indicates negative cycle is present as it should always be the case that `mat[i][i] = 0`
- to use on UG, every edge can be made into 2 edges of same weight facing opposite direction (DG)

If we use Dijkstra (src to all nodes) to calc multi-source multi-dest shortest paths, we have TC = `V * (E * log V)`, doing Dijkstra for every node `V` to get shortest path till every other node.

**Summary of Shortest Path Algorithms**:
|  Algorithm	|  TC  | Paradigm | Features
|---	|--- |---|---
|  BFS	|  `O(V + E)` | Systematic | Very Fast, works on both UG and DG
|  Dijkstra (PQ) | `O(E * log V)` | Greedy | Fast, no negatives, fails `shortest within k` problem
|  Dijkstra (Q)	| `O(E * V)` | Greedy | Unoptimized Dijkstra (bad, identify and avoid using ever)
|  Bellman-Ford	|  `O(E * V)` | DP | Negative weights and cycles
|  Floyd-Warshall |  `O(V*3)` | Brute-Force | Multi-source to multi-dest, negative weights and cycles

### Minimum Spanning Tree (MST)
A spanning tree is a tree in which we have `N` nodes (i.e. All the nodes present in the original graph) and `N-1` edges and all nodes are reachable from each other. If the sum of all edges is minimum possible it is an MST.

Multiple MST are possible for a graph and no formula can output number of possible MST since we don't have connected or not info without edges.

**Prim's Algorithm**:
- greedy to build MST or just get MST sum
- start at a arbitrary node and pick minimum edge to connect neighbours (using Min-heap PQ)
- use `vis[]` but mark visited on actually reaching the node (unlike BFS) rather than touching from a neighbour
- track and inc `sum` as sum of all edges of MST
- `node = {wt, {node, parent}}` - node and parent pair is added to MST list on finding a min edge between them

Since we're not marking nodes visited on touching from neighbour (as done in BFS) but rather on actually visiting, this may cause a case where a node is enqueued multiple times from neighbouring nodes (e.g. three node cycle). This can happen even when we're checking for visited status before enqueuing (since marking is done later). To mitigate this, upon reaching a node we check if it has already been visited and drop it in that case, otherwise process its neighbours and enqueue its neighbours if they're not visited.

Time Complexity Analysis:
```txt
explore every edge and for every edge pick minimum among them using PQ
E * log (heap_size)
E * log E
```
