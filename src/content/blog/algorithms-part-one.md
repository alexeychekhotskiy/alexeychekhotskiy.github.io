---
title: "Algorithms part 1"
description: "Algorithms: linear and binary search, quick, bubble, selection sort"
pubDate: "Jul 12 2022"
heroImage: "/algoritms-1/algoritms-1.png"
---

# Algorithms list

1. Linear search

    Linear search is a simple search algorithm that checks every element in a list or array one by one until it finds the target element or determines that the element is not present in the list.

    ```jsx
    const array = [1, 4, 5, 8, 5, 1, 2, 7, 5, 2, 11];
    let count = 0;
    function linearSearch(array, item) {
        for (let i = 0; i < array.length; i++) {
            count += 1;
            if (array[i] === item) {
                return i;
            }
        }
        return null;
    }

    console.log(linearSearch(array, 1));
    console.log("count = ", count);
    ```

    ![Untitled](/algoritms-1/linear_search.png)

    Linear search has a time complexity of O(n), where n is the number of elements in the list or array. This means that the time it takes to search for a target element increases linearly with the size of the input data. It is suitable for small lists or when the data is not sorted, but for larger datasets, binary search or other more efficient algorithms are preferred because they have better time complexities.

2. Binary search

    Binary search is a highly efficient algorithm for finding a specific value in a **sorted** array. It works by repeatedly dividing the search interval in half, effectively reducing the search space by half with each iteration until the desired element is found or the search interval becomes empty.

    ```jsx
    const array = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15];
    let count = 0;

    function binarySearch(array, item) {
        let start = 0;
        let end = array.length;
        let middle;
        let found = false;
        let position = -1;
        while (found === false && start <= end) {
            count += 1;
            middle = Math.floor((start + end) / 2);
            if (array[middle] === item) {
                found = true;
                position = middle;
                return position;
            }
            if (item < array[middle]) {
                end = middle - 1;
            } else {
                start = middle + 1;
            }
        }
        return position;
    }

    function recursiveBinarySearch(array, item, start, end) {
        let middle = Math.floor((start + end) / 2);
        count += 1;
        if (item === array[middle]) {
            return middle;
        }
        if (item < array[middle]) {
            return recursiveBinarySearch(array, item, 0, middle - 1);
        } else {
            return recursiveBinarySearch(array, item, middle + 1, end);
        }
    }

    console.log(recursiveBinarySearch(array, 0, 0, array.length));
    console.log(count);
    ```

    1. Start with the entire sorted array.
    2. Calculate the middle element of the current search interval.
    3. Compare the middle element with the target value you are searching for.
        - If the middle element is equal to the target, you have found the desired element, and the search is successful.
        - If the middle element is greater than the target, it means the target must be in the left half of the search interval.
        - If the middle element is less than the target, it means the target must be in the right half of the search interval.
    4. Repeat steps 2 and 3 with the appropriate half of the search interval, effectively halving the search space each time.
    5. Continue this process until you either find the target element or the search interval becomes empty (indicating that the target is not in the array).

    Binary search has a time complexity of O(log n), where n is the number of elements in the sorted array. This makes it very efficient, especially for large datasets, compared to linear search algorithms that have a time complexity of O(n).

3. Selection sort

    The selection sort algorithm is a sorting algorithm that works by repeatedly finding the minimum (or maximum) element from the unsorted part of the array and swapping it with the first unsorted element. **O(n^2) time because you have to check each item in the list (it takes O(n) time) and you have to do that n times**

    ![Untitled](/algoritms-1/binary_search.gif)

    ```jsx
    function selectionSort(arr) {
        let n = arr.length;

        // Helper function to find the index of the minimum value
        function findMinIndex(startIdx) {
            let minIdx = startIdx;
            for (let j = startIdx + 1; j < n; j++) {
                if (arr[j] < arr[minIdx]) {
                    minIdx = j;
                }
            }
            return minIdx;
        }

        for (let i = 0; i < n - 1; i++) {
            // Find the index of the minimum value
            let minIdx = findMinIndex(i);

            // Swap if the minimum isn't in the current position
            if (minIdx !== i) {
                let temp = arr[i];
                arr[i] = arr[minIdx];
                arr[minIdx] = temp;
            }
        }

        return arr;
    }

    // Example
    const arr = [64, 34, 25, 12, 22, 11, 90];
    console.log(selectionSort(arr)); // [11, 12, 22, 25, 34, 64, 90]
    ```

4. Bubble sort

    Bubble sort is a simple sorting algorithm that compares and swaps adjacent elements of an array until the array is fully sorted. This algorithm is named "bubble sort" because larger elements "bubble" to the end of the array with each iteration.

    ![Untitled](/algoritms-1/bubble_sort.png)

    ```jsx
    const arr = [
        0, 3, 2, 5, 6, 8, 23, 9, 4, 2, 1, 2, 9, 6, 4, 1, 7, -1, -5, 23, 6, 2,
        35, 6, 3, 32, 9, 4, 2, 1, 2, 9, 6, 4, 1, 7, -1, -5, 23, 9, 4, 2, 1, 2,
        9, 6, 4, 1, 7, -1, -5, 23,
    ];
    let count = 0;

    function bubbleSort(array) {
        for (let i = 0; i < array.length; i++) {
            for (let j = 0; j < array.length; j++) {
                if (array[j + 1] < array[j]) {
                    let tmp = array[j];
                    array[j] = array[j + 1];
                    array[j + 1] = tmp;
                }
                count += 1;
            }
        }
        return array;
    }

    console.log("length", arr.length);
    console.log(bubbleSort(arr)); // O(n*n)
    console.log("count = ", count);
    ```

    1. Start from the beginning of the array.
    2. Compare the first element with the next element. If the first element is greater than the second, swap them.
    3. Move on to the next pair of elements (the second and third), compare them, and, if necessary, swap them.
    4. Continue this comparison and swapping process until you reach the end of the array, at which point the largest element will have "bubbled" to the end.
    5. After the first iteration, the largest element will be at the end of the array. Then repeat the process for all the remaining elements in the array, and each time, the largest unsorted element will "bubble" into its correct position.
    6. Keep repeating this process for the entire array until no swaps are made on an iteration. This means the array is already sorted, and the algorithm terminates.

    However, bubble sort is not the most efficient sorting algorithm, especially for large arrays, because its worst-case time complexity is O(n^2), where n is the number of elements in the array. For larger data sets, more efficient sorting algorithms like Quick Sort, Merge Sort, or Heap Sort are preferred.

5. Quick Sort

Quick Sort is a popular and efficient sorting algorithm that uses a divide-and-conquer approach to sort an array or list of elements. It works by selecting a pivot element from the array and partitioning the other elements into two sub-arrays, according to whether they are less than or greater than the pivot. The sub-arrays are then sorted recursively.

![Untitled](/algoritms-1/quick_sort.png)

```jsx
const arr = [
    0, 3, 2, 5, 6, 8, 23, 9, 4, 2, 1, 2, 9, 6, 4, 1, 7, -1, -5, 23, 6, 2, 35, 6,
    3, 32, 9, 4, 2, 1, 2, 9, 6, 4, 1, 7, -1, -5, 23, 9, 4, 2, 1, 2, 9, 6, 4, 1,
    7, -1, -5, 23,
];
let count = 0;

function quickSort(array) {
    if (array.length <= 1) {
        return array;
    }
    //pivot can be a random but I choose the central element of the array
    let pivotIndex = Math.floor(array.length / 2);
    let pivot = array[pivotIndex];
    let less = [];
    let greater = [];
    for (let i = 0; i < array.length; i++) {
        count += 1;
        if (i === pivotIndex) continue;
        if (array[i] < pivot) {
            less.push(array[i]);
        } else {
            greater.push(array[i]);
        }
    }
    return [...quickSort(less), pivot, ...quickSort(greater)];
}

console.log(quickSort(arr));
console.log("count", count);
```

1. Choose a pivot element from the array. The choice of the pivot can vary, but a common strategy is to pick the first element, the last element, or a random element.
2. Partition the array into two sub-arrays: one with elements less than the pivot and one with elements greater than the pivot. Elements equal to the pivot can go in either sub-array.
3. Recursively apply Quick Sort to the sub-arrays. This means repeating the process for the sub-array of smaller elements and the sub-array of larger elements.
4. Combine the sorted sub-arrays and the pivot to get the final sorted array.

Quick Sort is efficient on average and has an average time complexity of O(n log n), where n is the number of elements in the array. However, in the worst-case scenario (when the pivot is consistently the smallest or largest element), Quick Sort can have a time complexity of O(n^2). To mitigate this, randomized or median-of-three pivot selection strategies can be used.
