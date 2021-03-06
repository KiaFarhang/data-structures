# Binary Heaps

[Implementation in TypeScript](https://gist.github.com/KiaFarhang/c508051a910feb91191bd1f716af63fd)

A type of tree, similar to a binary search tree - but with extra rules. Used commonly for priority queues and graph traversal.

For both types:

- There's no guarantee between siblings.
- As compact as possible; the children of each node are as full as possible and left children are filled out first.

## Max Binary Heap

Parent nodes are always larger than child nodes.

```
            20
          /   \ 
        11      15
      /  \     /  \
    3     6   10   8
```

## Min Binary Heap

Parent nodes are always smaller than child nodes.

```
            3
          /   \ 
        6       8
      /  \     /  \
    20    15   10   11
```


## Storing Heaps

You can actually use arrays/lists to store heaps because of a cool mathematical relationship. Squash the max heap above like so:

```
20,11,15,3,6,10,8
```

For any node at index `i`, the node's left child is at index `2i+1` and its right child is at `2i+2`.

For any node at index `i`, the node's parent is `floor(i-1)/2` (or use integer division).

## Adding to a Binary Heap

If you use an array to store a binary heap, you can always add values to the end - but you then have to _bubble_ the new value to its rightful place (usually with recursion).

Take this max heap example, where we add 35 to the end of the heap:

```
            20
          /   \ 
        11      15
      /  \     /  \
    3     6   10   35
```

We need to check 35's parent. Since it's 15, we should swap the two values:

```
            20
          /   \ 
        11      35
      /  \     /  \
    3     6   10   15
```

Next we check the next parent - it's only 20, so we should swap one more time:

```
            35
          /   \ 
        11      20
      /  \     /  \
    3     6   10   15
```

Now we've guaranteed we still have a max binary heap, as each parent is greater than its children.

## Extracting from the heap:

Extraction always removes the first item in the heap, then uses a simimlar recursive method to reorder the underlying array. Let's say we're going to extract from this tree:

```
            20
          /   \ 
        11      15
      /  \     /  \
    3     6   10   2
```

First we set 20 aside, then set the _last_ item to be the first.

```
            2
          /   \ 
        11      15
      /  \     /  
    3     6   10 
```

We're ready to return 20, but this binary heap isn't right - 2 is less than both of its children. We need to _sink_ the new top down, swapping it to where it belongs. So we look at its children and compare it with the greater of the two (or the lesser of the two if this was a min heap). Since 15 is greater, we swap:

```
            15
          /   \ 
        11      2
      /  \     /  
    3     6   10 
```

We do the same again to swap it with 10, so the heap is once again correct and we can return 20:

```
            15
          /   \ 
        11      10
      /  \     /  
    3     6   2 
```

**Special note**: Let's say the tree looked like this, and we were going to sink the 2 on the second level:

```
            15
          /   \ 
        11      2
      /  \     /  
    3     6   2 
```

In this case, **we want to swap 2 with its child, because the child entered the underlying array first. If the value is a tie, we want to put the chronologically first element earlier in the underlying array.**


# Priority Queues

A special type of queue in which the first element out is the first element inserted _with the highest priority._ Uses a binary heap to accomplish this.

[Implementation in TypeScript](https://gist.github.com/KiaFarhang/5159d4b6ef0db3d1cd5bea004f36fdc7)

## Big O Considerations

### Time Complexity

**Insertion: `O(logn)`**
**Removal: `O(logn)`**

Let's say we have 16 elements in our binary heap, laid out like so (remember the child-index calculation above):

```
100
  19 36
      17 12 25 5
          9 15 6 11 13 8 1 4
```

Say we want to insert `200` into the heap. This is the worst-case scenario; it must bubble all the way to the top. But we only need to make four comparisons - to 9, 17, 19 and 100 - to get it there. For sixteen elements, we have 4 comparisons. `n = 2^4`

If we had twice as many elements, we would only make one more comparison. `32 (n) = 2 ^5`

**Search: `O(n)`**

Let's say we wanted to find `1` in the heap above. Starting from the top, there's no rule like a binary search tree that lets us cut the tree in half and only look at half the elements. We know 1 is less than 100, which only tells us it _might_ be lower in the heap.

Binary heaps aren't great for searching; you'd want a binary search tree for that.