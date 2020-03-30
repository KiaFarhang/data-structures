# Hash Tables

[Implementation in TypeScript](https://gist.github.com/KiaFarhang/ea64a7bb6fe9d46aa3b16ce4fd919fe3)

A mapping of unordered key-value pairs. Very fast for finding, adding and removing values. Internally, hash tables use a _hashing function_ to convert keys (which can be strings, objects, files, anything) to numbers, so the value corresponding to a key can be stored and retrieved from an array at that numerical index.

## Hash Functions

Should be:

- Fast (constant time)
- Doesn't cluster outputs near certain indices, but distributes them uniformly
- Deterministic - the same input yields the same output every time

(Note none of this makes a hash function cryptographically secure; that's a whole different topic)

Hash functions often use prime numbers to avoid collisions, and it's especially helpful if the table/array length itself has a prime length.

## Handling Hash Collisions

What happens when your hash function returns the same index for two different keys? Two common approaches:

- **Separate chaining**: Store values at each table index _in another data structure_, like a subarray or a linked list.

- **Linear probing**: Any time the hash function returns an index already in use, it searches through the array starting at that index for the next empty index and returns _that_ index instead.

## Big O Considerations

### Time Complexity

Note that this assumes a good hash function that runs in constant time and evenly distributes data.

**Insertion**: `O(1)`
**Deletion**: `O(1)`
**Access**: `O(1)`
**Searching for values**: `O(n)`