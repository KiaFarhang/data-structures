# Trees

Hierarchical representation of data:

```
            A
          /   \ 
        B       C
      /  \     /  \
    D     E    F   G
```

In the above tree:

- A is the root
- D, E, F and G are leaves

## Traversal

### Breadth-First Search

- Starting from the root, process each vertical level of the tree from left to right. A, B, C, D, E, F, G

- Use a queue to visit/process nodes:

```typescript
if(this.root) {
    const queue: Queue<TreeNode<T>> = createQueue();

    queue.enqueue(this.root);

    while(queue.size > 0) {
        const currentNode: TreeNode<T> = queue.dequeue() as TreeNode<T>;
        action(currentNode);
        currentNode.children.forEach(child => queue.enqueue(child));
    }
}
```

### Depth-First Search

- Starting from the root, process the tree from top to bottom. Uses recursion to visit nodes. Each of the three flavors differ only in the order they visit nodes.


#### Depth-First Preorder

- Visit a node, then visit each child recursively. A,B,D,E,C,F,G

```typescript
if(this.root) {
    const recursiveFunction = (currentNode: TreeNode<T>, action: (node: TreeNode<T>) => void): void => {
        action(currentNode);
        currentNode.children.forEach(child => recursiveFunction(child, action));
    }

    recursiveFunction(this.root, action);
}
```

#### Depth-First Postorder

- Visit each child recursively, _then_ visit the node. D,E,B,F,G,C,A

```typescript
if(this.root) {
    const recursiveFunction = (currentNode: TreeNode<T>, action: (node: TreeNode<T>) => void): void => {
        currentNode.children.forEach(child => recursiveFunction(child, action));
        action(currentNode);
    }

    recursiveFunction(this.root, action);
}
```


#### Depth-First Inorder

- Works if each node has at most two children. Visit the left child, then the node, then the right child.


## Considerations

- Visiting every node is `O(n)` for both breadth- and depth-first search.
- Breadth-first search's space complexity gets messy with wide trees - you have to store a lot in that queue.
- Conversely, breadth-first search's space complexity is great for long, skinny trees - you barely keep anything in the queue.
- DFS Inorder is useful for binary search trees - you'll get nodes in their underlying order, like 3,6,8,10,11,15,20 here:

```
            10
          /   \ 
        6       15
      /  \     /  \
    3     8   11   20
```

- Preorder is good if you want to export/store a serialized tree for reconstruction later.