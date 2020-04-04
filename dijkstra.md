# Dijkstra's Shortest Path Algorithm

[Implementation in TypeScript](https://gist.github.com/KiaFarhang/356d93644ff01c6323459a373e8b3cb2)

Given a weighted graph, finds the shortest path between two vertices. Hugely influential - used in GPS, networking, etc.

## Walkthrough

Dijkstra's algorithm uses a priority queue and a few maps. Say we have this graph, and we want the shortest path between A and F:


```
        6
    B ------D---\  2
 1/        /     \
A          |3     F
  2\       |     /
    C-----E-----/4
       4
```

The shortest path is A->B->D->F, a total distance of 9.

To start the problem, we set up our data structures. The distance map tracks the shortest known distance from the start to each of the vertices.

```typescript
{
    'A': 0,
    'B': Infinity,
    'C': Infinity,
    'D': Infinity,
    'E': Infinity,
    'F': Infinity,
}
```
At the beginning, we only know the starting vertex's distance to itself - 0 - so we set every other vertex's shortest distance to the largest possible number.

The priority queue, when polled, will always give us the vertex with the shortest known distance to the start.

| Vertex        | Shortest Distance to Start    
| ------------- |-------------|
| A             | 0             | 

Next we need a map of every vertex to the vertex that should come before it, if we're traveling from start to end as efficiently as possible. We don't know this path yet, so our starting map looks like this:

```typescript
{
    'A': null,
    'B': null,
    'C': null,
    'D': null,
    'E': null,
    'F': null,
}
```

Lastly we just need a map to keep track of which vertices we've visited:

```typescript
{

}
```

## Running the algorithm

We start by pulling from the priority queue, which will give us the starting vertex. Then we follow this loop:

```
while (current vertex !== null && current vertex !== end vertex)
    add this vertex to the map of visited vertices
    for each vertex V connected to this one
        if V has not been visited
            calculate D, the sum of current vertex's distance (i.e. distances.get(current vertex)) + V.weight
            if D < distances.get(V)
                update distances map to D - the new shortest distance from start to V
                update previous map to set V's previous to the current vertex
                enqueue(V, D)
    
    current vertex = queue.dequeue()
```

After processing A, the starting vertex, our data structures look like this:

| Vertex        | Shortest Distance to Start    
| ------------- |-------------|
| B             | 1             | 
| C             | 2             | 
| B             | Infinity      | 
| C             | Infinity      | 
| D             | Infinity      | 
| E             | Infinity      | 
| F             | Infinity      | 

```typescript
{
    'A': null,
    'B': 'A',
    'C': 'A',
    'D': null,
    'E': null,
    'F': null,
}
```

```typescript
{
    'A': true
}
```

So we pull B off the queue, and after one more round we get:

| Vertex        | Shortest Distance to Start    
| ------------- |-------------|
| C             | 2             | 
| D             | 7             | 
| B             | Infinity      | 
| C             | Infinity      | 
| D             | Infinity      | 
| E             | Infinity      | 
| F             | Infinity      | 

```typescript
{
    'A': null,
    'B': 'A',
    'C': 'A',
    'D': 'B',
    'E': null,
    'F': null,
}
```

```typescript
{
    'A': true,
    'B': true
}
```

And so on until we reach F.

After the loop, the relevant part of the previous map will look like this:

```typescript
{
    'A': null,
    'B': 'A',
    'D': 'B',
    'F': 'D',
}
```

To construct the shortest path, we start at the ending vertex and follow the map backwards until we reach `null`: `F - D - B - A`.

Note this is backwards - you either need to reverse the list or push everything onto a stack as you run through the map, then continually pop off the stack to construct your list. I think the stack approach is better time complexity.