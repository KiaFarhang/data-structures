# Dynamic Programming

Solving a problem by breaking it down into smaller problems, solving those subproblems and storing their solutions. Used in very specific situations and, when it can be used, often dramatically improves performance.

Dynamic programming works on problems containing **overlapping subproblems** and an **optimal substructure**.

## Overlapping Subproblems

A problem has overlapping subproblems if we can break it down into pieces that are used more than once. Take the Fibonnaci sequence, where every number after the first two is the sum of the two numbers before it:

1, 1, 2, 3, 5, 8, 13, 21, etc.

Let's say we want to calculate `fib(5)`:

```
                           fib(5)
                fib(4)      +       fib(3)
            fib(3) +  fib(2)  fib(2) + fib(1)
        fib(2) + fib(1)
```

We have subproblems here - `fib(5)` is just the sum of `fib(4)` and `fib(3)`.

But we also have _overlap_: We calculate `fib(3)` twice here, in both sides of the problem.

Something like merge sort, on the other hand, contains subproblems.

If we wanted to merge sort `[15, 13, 4, 9]`, we'd go like this:

`mergeSort([15, 13])`
`mergeSort([15])`
`mergeSort([13])`

And then sew the left side back up to `[13, 15]`.

We'd do the same on the right side, to get `[4, 9]`.

Finally, we'd merge everything together as `[4, 9, 13, 15]`.

We broke the problem into subproblems, but _those subproblems were never reused_. Merge sort is **not** a good candidate for dynamic programming. If you _only_ have subproblems, dividing and conquering is usually the better approach.

## Optimal Substructure

A problem has an optimal substructure if the best solution for the problem is a combination of the best solutions to its subproblems.

Think about the shortest path between two vertices on a weighted graph:

```
A -2- B         D
      |  \      |
      2    \3   1
      |      \  |
      E - - - - C  
            2
```

The shortest path from A to D is `A - B - C - D`. That contains the shortest path from A to C (`A - B - C`) and shortest path from A to B (`A - B`).

## An Example

Let's look at the Fibonacci sequence again. A typical recursive solution might look something like this:

```typescript
const fib = (num: number): number => {
    if (num <= 2) return 1;

    return fib(num - 1) + fib(num - 2);
}
```

There's actually a glaring problem with this solution that starts kicking in fairly soon (around the 30th-40th number in the sequence). As we discussed in an earlier section, the subproblems used to calculate the answer _overlap_.

If we pass 40 to the function above, the left side of the return statement will calculate `fib(39)`. That, in turn, will calculate `fib(38)`, `fib(37)`, and so on.

The problem is the right side of the original function will _also calculate `fib(38)`_, even though we already did it on the left side! All this duplicate work means **the naive recursive solution to this problem has a time complexity close to `O(2^n)`, which is so bad it should be illegal.**

How can dynamic programming solve the problem more efficiently? Well...

## Memoization

Memoization is the act of storing the result of expensive function calls, so we can return the cached result when the function is called with the same output.

Here's a solution to the Fibonacci problem using memoization:

```typescript
const dynamicFib = (num: number): number => {
    const fibStorage = new Map<number, number>();
    fibStorage.set(1, 1);
    fibStorage.set(2, 1);

    const helper = (num: number): number => {
        const fromStorage = fibStorage.get(num);

        if (fromStorage) return fromStorage;

        const total = helper(num - 1) + helper(num - 2);

        fibStorage.set(num, total);

        return total;
    }

    return helper(num);
}
```

If passed 40, this function will still have to calculate `fib(39)`, `fib(38)`, etc. But it'll only do the expensive computational work one time, storing the result of each call in a map. Every other time the function needs to get `fib(38)`, it'll just do a constant-time lookup to get the value it already computed. **The right side of every tree in the calculation will be constant time, as the left side will have already calculated the value. This solution is roughly `O(n)`.**

## Tabulation

If our memoization solution was top-down, tabulation is a bottom-up approach to solving dynamic programming problems. It's typically done iteratively, storing results in a table/array. It can lead to better space complexity, because you don't store a ton of recursive function calls on the stack.

Here's a solution to the Fibonacci problem using tabulation:

```typescript
const tabularFib = (num: number): number => {
    const results: number[] = [];

    for (let i = 0; i < num; i++) {
        if (i <= 1) {
            results.push(1);            
        } else {
            results.push(results[i-1] + results[i-2]);
        }
    }

    return results[num-1];
}
```

This also performs in `O(n)` time. But at high values of `n`, the memoized version will trigger stack overflows. This version has much better space complexity and will perform fine.