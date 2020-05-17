# Sorting

Pretty simple - sorting algorithms take a list and return a list with the elements sorted. Many common algorithms sort _in place_ - they modify the existing array as they sort.

## Bad

All of these algorithms have the same (awful) time complexity:

**Time**: O(n^2) (nested loops!)
**Space**: O(1)


### Bubble Sort

Run through the list over and over again, "bubbling" the biggest elements to the end. Repeat the process until you can run through the list without making a swap - that's how you know the list is sorted

```
 |  x
[8, 5, 2, 3] 8 is bigger than 5, so we should swap

    |  x
[5, 8, 2, 3] 8 is bigger than 2, so we should swap

       |  x  
[5, 2, 8, 3] 8 is bigger than 3, so we should swap

 |  x
[5, 2, 3, 8] 5 is bigger than 2, so we should swap

    |  x
[2, 5, 3, 8] 5 is bigger than 2, so we should swap

// repeat until you run through the whole list without swapping

```

```javascript
function bubbleSort(array) {
    let madeASwap = false;
    let rightPointer = array.length - 1;
    do {
        let swapsDone = 0;
        for (let i = 0; i < rightPointer; i++) {
            if (array[i] > array[i + 1]) {
                swap(array, i, i + 1);
                swapsDone++;
            }
        }
        rightPointer--;
        madeASwap = swapsDone > 0;
    } while (madeASwap);
    return array;
}
```

### Selection Sort

Builds a sorted list one element at a time.

Starting at the beginning of the list, scan the rest of the list and find the smallest value. Swap that with the current item. Then move to the next item and start over again.

```
 |
[8, 5, 2, 3] // index of smallest item is 0
 |  x
[8, 5, 2, 3] // index of smallest item is 1
 |     x
[8, 5, 2, 3] // index of smallest item is 2
 |        x
[8, 5, 2, 3] // index of smallest item is 2
 |
[2, 5, 8, 3] // Swap items at current index and smallest index
    |
[2, 5, 8, 3] index of smallest item is 1.

// Repeat

```

```typescript

const selectionSort = (array: number[]): number[] => {
    for (let i = 0; i < array.length - 1; i++) {
        let indexOfSmallestValue = i;
        for (let j = i + 1; j < array.length; j++) {
            if (array[j] < array[indexOfSmallestValue]) {
                indexOfSmallestValue = j;
            }
        }

        swap(array, i, indexOfSmallestValue);
    }

    return array;
}
```
