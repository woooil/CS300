# Ch. 8 Graph

##  Graph
* $G = (V, E)$
  * $V$: Set of *vertices* (*nodes*)
  * $E$: Set of *edges*, pairs of vertices
* Directed graphs (Digraphs) vs. undirected graphs
* $|E| = O(V^2)$
* If $G$ is connected, $|E| \geq |V| - 1$
* "$G$ is *sparse*" $\equiv$ "$|E|$ is close to $|V|$"
* "$G$ is *dense*" $\equiv$ "$|E|$ is close to $|V|^2$"

## Adjacency Matrix
* An adjacency matrix of $G = (V, E)$, where $V = \{1, 2, ..., n\}$, is a $n \times n$ matrix $A$
  * $
    A[i, j] =
    \begin{cases}
      1 & \text{if}~ (i, j) \in E \\
      0 & \text{otherwise}
    \end{cases}
    $
* Uses $\Theta(V^2)$ storage; dense representation

## Adjacency List
* An adjacency list of $v \in V$ is the list $Adj[v]$ of vertices adjacent to $v$
* $
  |Adj[v]| = 
  \begin{cases}
    degree(v) & \text{if}~ G ~\text{is undirected} \\
    out-degree(v) & \text{if}~ G ~\text{directed}
  \end{cases}
  $
* $\sum_{v \in V} degree(v) = 2 |E|$
* $\sum_{v \in V} out-degree(v) = |E|$
* Uses $\Theta(V + E)$ storage; sparse representation

## Explore
```
procedure explore(G, v)
Input: G = (V, E); v is in V
Output: visited(u) is set to true for all nodes u reachable from v

  visited(v) <- true
  previsit(v)                   // optional
  for (v, u) in E do
    if not visited(u) do
      explore(u)
  postvisit(u)                  // optional
```
* Visits only the portion reachable from the starting point

## Depth-First Search
* Restarts from unvisited vertices
```
procedure dfs(G)
  for v in V do
    visited(v) <- false
  for v in V do
    if not visited(v) do
      explore(G, v)
```
* Running time: $O(|V| + |E|)$

## Connectivity in Undirected Graphs
* "An undirected graph is *connected*" $\equiv$ \
  "There is a path btw any pair of vertices"
* `ccnum[v]`: The number of connected components to which it belongs

```
procedure previsit(v)
  ccnum[v] <- cc
```
  * `cc`: Initialized to `0` and incremented each time called

## Previsit and Postvisit Orderings
* For each vertex, record the time of first discovery and final departure
```
procedure previsit(v)
  pre[v] <- clock
  clock <- clock + 1
```
```
procedure postvisit(v)
  post[v] <- clock
  clock <- clock + 1
```
* `clock` set to `1` initially
* For any nodes `u` and `v`, `[pre(u), post(u)]` and `[pre(v), post(v)]` are either disjoint or one is contained within the other.

## Types of Edges in DFS Tree
* Tree edges: edges of DFS forest
* Forward edges: From a node to a nonchild descendant
* Back edges: From a node to a nonparent ancestor
* Cross edges: From a node to a neither descendant nor ancestor

## Pre/Post ordering and Edge Types
* "`u` is an ancestor of `v`" $\equiv$ \
  "`v` is discovered during `explore(u)`"
* `pre[u]` $<$ `pre[v]` $<$ `post[v]` $<$ `post[v]`: Tree/forward
* `pre[v]` $<$ `pre[u]` $<$ `post[u]` $<$ `post[v]`: Back
* `pre[v]` $<$ `post[v]` $<$ `pre[u]` $<$ `post[v]`: Cross

## Directed Acyclic Graphs
* Directed acyclic graphs(dag): Directed graph w/o cycle
* Directed graphs have a cycle if and only if its DFS reveals a back edge

### Topological sort of a dag
* Linear ordering of vertices s.t. `u` appears before `v` for `(u, v) in E`
* There may be one or more possible linearizations
* Sink: Vertices w/ smallest post number
* Source: Vertices w/ highest post number

### Strongly Connected Components
* Strongly connected components (SCC): Sets of vertices that every vertex is reachable from every other vertex
* Meta-graph
* If we expore on a node in a sink, then we get the sink SCC
* Node with the highest post number lies in a soruce SCC
* If `C` and `C'` are SCCs and there is an edge from a node in `C` to a node in `C'`, then the highest post number in `C` is bigger than one in `C'`
  * We can linearize SCC in descending order of their highest post numbers

### Reverse Graph
* $G^R$: Graph in which all edges of $G$ is reversed
* $G$ and $G^R$ have the same SCCs