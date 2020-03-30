# Graphs

[Implementation in TypeScript](https://gist.github.com/KiaFarhang/4fc2dbff466b4535aa4ca27553875167)

A collection of nodes and the connections between them.

- **Vertex**: a node
- **Edge**: a connection between nodes

## Directed vs. Undirected

Undirected graphs have no directions to their edges; all edges are two-way connections. `A - B` lets you go from A to B _and_ vice versa. (Think becoming friends with someone on Facebook - you get their posts; they get yours)

In a directed graph, each vertice has a direction. `A -> B` only lets you move from A to B, not the other way around. (Think following someone on Twitter - You see their tweets; they don't see yours)

## Weighted vs. Unweighted

Edges in weighted graphs have some value assigned to the edges - e.g. the distance in miles between the two edges.

## Representing/Implementing Graphs

```
    A-------\
 /           B
F           |
  \          |
   \       / C
    \  /--D
    E    
```

(I'm not an art major)

### Adjacency Matrix

Use an array of arrays to store whether one vertex is connected to another:

```
-  A  B  C  D  E  F
A  0  1  0  0  0  1
B  1  0  1  0  0  0
C  0  1  0  1  0  0 
D  0  0  1  0  1  0
E  0  0  0  1  0  1
F  1  0  0  0  1  0
```

### Adjacency List

Store arrays/lists of each vertex's connections, oftentimes in a hash table where the key is the vertex or some property on it.

```
{
    A: [B, F],
    B: [A, C],
    C: [B, D],
    etc.    
}
```

### Big O Considerations

`|V|` - number of vertices
`|E|` - number of edges

| Operation     | Adjaceny List | Adjacency Matrix  |
| ------------- |:-------------:| -----:|
| Add Vertex    | O(1)        | O(V^2)|
| Add Edge      | O(1)        | O(1)|
| Remove Vertex | O(V+E)      | O(V^2) |
| Remove Edge   | O(E)        |  O(1)  |
| Query         | O(V+E)      |  O(1)  |
| Storage       | O(V+E)      | O(V^2) |

#### Adjacency Lists...

- Can take up less space in sparse graphs
- Are faster to iterate over all edges
- Can be slower to look up specific edges

#### Adjacency Matrices...

- Can take up more space in sparse graphs
- Are slower to iterate over all edges
- Are faster to look up specific edges

**Most graphs in the real world are sparse - there may be many nodes, but not a ton of connections. Adjacency lists are better for this use case. Matrices are better with tons and tons of edges.**


## Graph Traversal

### Depth-First

Explore as far as possible down one route before backtracking. For example, with this graph:

```
      A
     /  \
    B    C
    |    |
    D----E
     \  /
      F   
```

A depth-first traversal starting at A would visit B, D, E, C and F. It moves from D to E simply because E is "first" in D's adjacency list. Then it does the same thing to choose C from E before F.

You can do this with a recursive function:

```
Keep track of the vertices visited
For the given vertex
    Add it to the vertices visited
    For each of its connected vertices
        If the vertex has not been visited
        Call the recursive function on that vertex
```

### Breadth-First

Explore each of the vertices at a given "level" (AKA how removed it is from the starting vertex) before moving to the next level. In the graph above, breadth-first traversal would visit A, B, C, D, E and F.

Typically done with a queue.

```
Keep track of vertices visited
Push the starting vertex into the queue
While there's something in the queue
    If the vertex has not been visited
        Take whatever action
    Add vertex to vertices visited
    For each connected vertex
        If it's not been visited yet
        Add it to the queue
    Move to the next item in the queue
```