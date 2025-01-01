## Sorting

### Selection Sort

```ts
function selectionSort(arr) {
    let arrLength = arr.length;
    for(let i = 0; i< arrLength; i++){
        let minIndex = i;
        for(let j=i+1; j<arrLength; j++){
            if(arr[minIndex] > arr[j]) {
                minIndex = j
            }
        }
        if(minIndex !== i)
        [arr[i], arr[minIndex]] = [arr[minIndex], arr[i]]
    }
    
    return arr;
}
```


#### **How It Works:**
Selection Sort is a comparison-based sorting algorithm. It divides the array into two parts: a sorted portion and an unsorted portion. The algorithm repeatedly selects the smallest (or largest, for descending order) element from the unsorted portion and swaps it with the first element of the unsorted portion, incrementally growing the sorted portion.

#### **Time Complexity:**
- **Best Case:** O(n²)  
  Even if the array is already sorted, Selection Sort performs O(n²) comparisons because it always looks for the minimum element in the unsorted portion.
  
- **Worst Case:** O(n²)  
  The algorithm always scans the unsorted portion completely, regardless of its initial state.
  
- **Average Case:** O(n²)  
  The number of comparisons remains the same for any input because Selection Sort doesn't take advantage of partially sorted arrays.

#### **Space Complexity:**
- **Space Complexity:** O(1)  
  Selection Sort is in-place and requires no extra space apart from a few variables for swapping and indexing.

#### **When to Use Selection Sort:**
1. **Small Input Size:**  
   It is practical for small datasets where the simplicity of implementation outweighs its inefficiency.
   
2. **Memory Constraints:**  
   It is useful when memory is limited since it works in-place without additional memory overhead.
   
3. **Partially Ordered Output Needed:**  
   If the minimum (or maximum) element is needed quickly, Selection Sort guarantees that after each pass, one element is correctly placed.

#### **When Not to Use It:**
Avoid using Selection Sort for large datasets due to its poor time complexity compared to more efficient algorithms like Quick Sort, Merge Sort, or even Insertion Sort for moderately sized arrays.



## Bubble sort

```ts
function BubbleSort(arr) {
    let arrLength = arr.length;
   for(let i= 0 ; i< arrLength; i++ ){
       let swapped = false;
       for(let j= 0; j< arrLength - i - 1; j++) {
           if(arr[j] > arr[j+1]){
               [arr[j], arr[j+1]] = [arr[j+1], arr[j]];
               swapped = true;
           }
       }
       if(!swapped) {
           break;
       }
   }
    return arr;
}
```
In the bubble sort the inner loop keeps checking adjacent elements in the array and swaps them if they are out of order. If no swaps are made during an iteration, it means the array is already sorted, so the algorithm breaks out of the outer loop. With each iteration of the outer loop, the largest unsorted element is placed in its correct position at the end of the array. This is why, after each outer loop iteration, the algorithm checks one fewer element at the last position.



#### **Time Complexity:**
- **Best Case:** O(n)  
  Occurs when the array is already sorted, and the algorithm exits after one pass.
  
- **Worst Case:** O(n²)  
  Happens when the array is sorted in reverse order, requiring maximum comparisons and swaps.
  
- **Average Case:** O(n²)  
  On average, the algorithm requires quadratic comparisons and swaps.

#### **Space Complexity:**
- **Space Complexity:** O(1)  
  Bubble Sort is an in-place algorithm, requiring no additional memory.

#### **When to Use Bubble Sort:**
1. **Educational Purposes:**  
   Its simplicity makes it ideal for learning sorting algorithms and understanding basic concepts.
   
2. **Small Datasets:**  
   Works reasonably well for small arrays where performance is not critical.
   
3. **Nearly Sorted Data:**  
   The optimization with the `swapped` flag makes it efficient for already sorted or nearly sorted arrays.

#### **When Not to Use It:**
Avoid using Bubble Sort for large datasets as it is significantly slower compared to more efficient algorithms like Quick Sort, Merge Sort, or Heap Sort.


## Merge sort

```ts
function merge(leftSortedArr, rightSortedArr) {
    let leftCounter = 0;
    let rightCounter = 0;
    let result = [];

    while (leftCounter < leftSortedArr.length && rightCounter < rightSortedArr.length) {
        if(leftSortedArr[leftCounter] < rightSortedArr[rightCounter] ) {
            result.push(leftSortedArr[leftCounter]);
            leftCounter++;
        }
        else {
            result.push(rightSortedArr[rightCounter]);
            rightCounter++;
        }
       
    }
     return [...result, ...leftSortedArr.slice(leftCounter), ...rightSortedArr.slice(rightCounter)]
}

function mergeSort(arr) {
    // base case
    if(arr.length <=1) return arr;
    let mid = Math.floor(arr.length / 2);
    let leftSortedArr = mergeSort(arr.slice(0, mid ));
    let rightSorteArr = mergeSort(arr.slice(mid));

    // now empty the call stack by sorting them
    return merge(leftSortedArr, rightSorteArr)
    
}
```

Merge Sort keeps dividing the array into two parts recursively by splitting it in half until each array has just one element. Then, it starts merging the arrays back together until the call stack from the recursion is completely emptied. While merging two sorted arrays into one sorted array, it iterates through both arrays and creates a result array. If one of the arrays finishes first, the remaining elements of the other array are directly concatenated to the result array.

### When to Use Merge Sort:
Merge Sort is good when we need a stable sorting algorithm or when we are working with large datasets that need predictable performance. It’s also useful for sorting linked lists because it doesn’t need random access to elements. But it’s not great when memory is tight because it uses extra space for temporary arrays during merging.

### Complexity of Merge Sort:
- **Time Complexity:**
  - Best Case: O(n log n)
  - Worst Case: O(n log n)
  - Average Case: O(n log n)
  It always splits the array into halves and merges them back, so the time complexity is the same for all cases.

- **Space Complexity:**
  - Space: \(O(n)\)  
  It needs extra space for temporary arrays during the merging process.


