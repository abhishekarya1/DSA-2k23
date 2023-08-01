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
- Degree of a graph is twice the number of edges, since each edge connects two nodes: `deg(G) = 2 * nEdges`
- A Tree is nothing but an acyclic graph (can be interpreted as directed or undirected). Linked List and Heaps are also special cases of a graph.

## Representations
Adjacency: two nodes are adjacent only when they are connected by an edge.

- **Adjacency Matrix**: matrix of size `N x N` (`N` is number of nodes). Mark existing edge using `1` in the matrix coordinates. SC = `O(N^2)`.
- **Adjacency List**: for every node, store all its neighbours in a corresponding list. Use an array of vectors - `vector<int> adjList[n]`. SC = `O(2 * E)` for undirected graphs. Much more space efficient than matrix.

For weighted graphs, we can store weight `W` of an edge as `adj[u][v] = W` in adjacency matrix. In adjacency list, use `vector<pair<int, int>> adjList[n]` wehere pair's second element denotes weight of the edge.
