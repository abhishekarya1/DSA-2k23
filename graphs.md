A graph is a non-linear data structure comprised of a set of vertices `V` and a set of edges `E`.

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
- A graph can have any number of cycles, even zero.
- A Tree is nothing but an acyclic graph (can be interpreted as directed or undirected). Linked List and Heaps are also special cases of a graph. 

_Ref_: https://medium.com/free-code-camp/i-dont-understand-graph-theory-1c96572a1401#0cd4
